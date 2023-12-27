---
layout: post
title: An Intraday Momentum/Mean-Reversion Strategy on TSE ETFs.
category: quant
excerpt: "In this post, I formulate & backtest a momentum/mean-reversion strategy on several weeks of second-tick TSE ETF bid-ask data. It consists of two strategies: one to capture a lunchbreak momentum effect, and a Bollinger-Band/MAC strategy to capture noticed stationarity in the afternoon trading sessions." 

---
<!-- In this post, I formulate & backtest a momentum/mean-reversion strategy on several weeks of second-tick TSE ETF bid-ask data. It consists of two strategies: one to capture a lunchbreak momentum effect, and a Bollinger-Band/MAC strategy to capture noticed stationarity in the afternoon trading sessions. -->

I adapted this post from a HFT firm take-home test. Given 2 weeks of second-tick TSE ETF (1570.T) price data, I was to backtest a strategy based off predicting mid (1/3/5s) ahead. However, predicting mid is difficult & prices don't move enough at 5s horizons to cover spread costs for a directional strategy.


<!-- <div class="image-container">
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/tweets/tweet_cephalopod_1.png" alt="First Image" class="side-by-side" height="175" width='400'>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/tweets/tweet_nopeitslily_1.png" alt="Second Image" class="side-by-side" height="175" width="400">
</div> -->

<!-- <center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/tweets/tweet_cephalopod_1.png" alt="First Image" class="side-by-side" height="175">
</center> -->
<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/tweets/tweet_cephalopod_1.png" alt="First Image" class="side-by-side" height="220">
</center>

As post-mortem, I revamped my approach to something much simpler/ML-free (exponential/simple moving averages), based off QuantTwit wisdom above. I analyze the data, uncover two patterns/effects, then backtest two strategies based off these patterns. One performs well, the other performs poorly. I then takeaway some learning points.

The repo/code for the project is [here](https://github.com/ryanczm/TSE-ETF-Mean-Reversion-Momentum). Challenging parts were: 1. Coding up a stoploss, 2. Parallelizing backtest logic to ensure each each day's prices were kept separate in the backtest and 3. Ensuring signals were converted to positions in correct fashion.

## Data Analyis 

<!-- ### Why My Short Horizon Directional Strategy Failed -->


<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/1_spreads.png">
  <figcaption>Price movements at different interval lengths for the morning session (9.30-11.30am) on 12th July of the 1570.T ETF on the Tokyo Stock Exchange</figcaption>
</center>


After some data cleaning, price changes at different intervals are plotted. This showcased the variance scaling property of Brownian motion $dW_t \propto \sqrt{dt}$  $\space $ or $\space$    $Var[W_{t+s}-W_t]=s$: variance in price between two points in time increases proportionally with the length of time passed.

This meant a directional strategy at short horizons would be unprofitable as any round trip profit would be negated by the cost of crossing the spread. This is seen by the red lines at -5 and 5 (the 90th percentile of spread values). At 5s intervals, majority of price movements are within $\pm$ yen. While the firm knew this, they prompted me to do a directional strategy as I had no MM experience.


The data is split into in-sample (first 4 trading days: 12-14 & 18 July) and out-of-sample (last 5 trading days: 19-24 July) periods. There are significant jumps from close of previous day to the open of next, and the inverse ETF effect is noticeable:

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/2_overall_price.png">
  <figcaption>ETF prices across entire period.</figcaption>
</center>


The Tokyo Stock Exchange has two trading sessions: 9.30-11.30am, an hour lunch break, then an afternoon session from 12.30 to 3.00pm.  

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/3_in_sample_daily_prices.png">
  <figcaption>Daily prices across in-sample period. A red line is drawn at 11am.</figcaption>
</center>

From these plots, morning sessions had more volatility/trend, while afternoon sessions seemed more stationary with less trend. The barchart below demonstrates the morning-afternoon vol difference.

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/5_am_pm_vol.png" height="350">
  <figcaption>Vol differences in AM vs PM trading sessions (in-sample).</figcaption>
</center>


Momentum before lunch seemed to "carry over" into lunch from eyeballing the time series. A scatter of 30m pre-lunch momentum (price change from 11-11.30am) against lunch break momentum (price change from 11.30-12.30pm) across the 4-day in-sample period shows this: 

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/6_lunch_momentum.png" height="350">
 </center>

(Disclaimer: I am aware my dataset is too small to properly investigate if these effects are significant or due to chance/randomness. However, for learning purposes, I ignore this and try to build strategies to capitalize on these effects).

So, two effects were noticed: lunch momentum and lack of trend in afternoon. Here is the cleaned price data:

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/dataframes/2_df_1570.png" alt="First Image" class="side-by-side" height="290">
  <figcaption>Cleaned second-tick price data</figcaption>
</center>


## Strategy & Ideation

No matter the approach (train/test, groupby time, rolling window), ML models could never perform better than a generalized dice roll. Hence, using a simpler method might fare better.

For the lunchtime effect, pre-lunch 1h momentum was used as a signal to trade across lunch. 

 A modified Bollinger bands/mean reversion (BB) strat was used for the afternoon trading session: the bands were formed by a longer EWMA window ($n$), entering and exiting when a shorter EWMA ($m$) crossed over. Initially, out-of-sample.performance was too good to be true, I later realized this was because there was no stop loss implemented, which was fixed.

To decide on $n$, we use the half-life of mean-reversion of midprice modelled as an Ornstein Uhlenbeck process (Jansen/Chan): an approximate time for a series to revert after a deviation, calculated as ~3100s or 50 minutes.

For the remaining free parameters $m$ and $k$ and the stop loss duration, cumulative PnL & round trip statistics for each parameter set were recorded via grid search on an in-sample period, then applied to the out-of-sample period.


<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/dataframes/4_df_bb_params.png" alt="First Image" class="side-by-side" height="290">
  <figcaption>Best in-sample parameters, ranked by cumpnl.</figcaption>
</center>



I then chose the parameters with highest in-sample cumulative PnL and apply it out-of-sample. Unfortunately, this was an example of overfitting and fared poorly out-of-sample.

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/dataframes/5_df_bb_test.png" alt="First Image"  height="53">
  <figcaption>Bollinger band strategy, out-of-sample performance, with best in-sample parameters.</figcaption>
</center>


Fortunately, the lunchbreak strategy did well. Using the sign of pre-lunch momentum to make a trade is profitable as long as if all the points are in the top-right and bottom-left quadrants. In this case, the signal captured the correct price movement.

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/9_oos_lunch_momentum.png" alt="First Image" height="400">
</center>

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/dataframes/dataframe_pre_lunch_momentum_out_sample.png" alt="First Image"  height="150">
  <figcaption>Out-sample performance of the lunch break strategy.</figcaption>
</center>


Obviously, to test this idea properly, much more data points would be needed. Finally, the entry/exit points and cumulative PnL of the Bollinger-band/mean-reversion strategy were plotted:

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/7_oos_signals.png" >
  <figcaption>Out-sample performance of the Bollinger band strategy.</figcaption>
</center>

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/7_is_signals.png" >
  <figcaption>In-sample performance of the Bollinger band strategy.</figcaption>
</center>

It seemed that the in-sample strategy was very profitable in the first half an hour when the ETF exhibited mean-reverting behavior, but 
The out-of-sample data seems less stationary than in-sample, calling into question the hypothesis.

<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/8_oos_performance.png" >
  <figcaption>Out-of-sample performance of the Bollinger band strategy.</figcaption>
</center>
<center>
  <img src="{{ site.imageurl }}/JapaneseEtfStrat/graphs/10_is_performance.png" >
  <figcaption>In-sample performance of the Bollinger band strategy.</figcaption>
</center>

And there we have it!

## Conclusion

To conclude, analysis was done to notice patterns within the dataset: afternoon sessions being more stationary, and a pre-lunch momentum effect that carried over into lunch break. The former performed poorly while the latter performed well out-of-sample.

 Overall, I have these takeaways which I've learnt fumbling around through this project:

1. Choosing parameters by optimizing via a grid search is an easy way out, but the more you do it, the higher the chance of overfitting, as seen by the Bollinger band strategy.
2. It seems better to have evidence-backed rationales for making a decision/choosing parameters than just using brute force, as seen by the lunch-break strategy.

The code for this project can be found [here](https://github.com/ryanczm/Quant/blob/master/1.%20TSE%20ETF%20Mean-Reversion%20Momentum%20Strategy/_strategy_backtest.ipynb).