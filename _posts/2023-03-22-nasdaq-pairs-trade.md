---
layout: post
title: A NASDAQ Pairs Trade
category: quant
excerpt: "In this post, I implement a simple pairs trading strategy based on bivariate cointegration tests in the selection phase on NASDAQ 100 equities. After doing the project, I realized why this was a commonly memed-on approach on QuantTwit: brute-forcing cointegration tests is the fastest route to multiple comparison bias."

---

In this post, I implement a simple pairs trading strategy based on bivariate cointegration tests in the selection phase on NASDAQ 100 equities. . After doing the project, I realized why this was a commonly memed-on approach on QuantTwit: brute-force cointegration tests is the fastest route to multiple comparison bias.

The repo/code for this project is [here](https://github.com/ryanczm/Quant/tree/master/2.%20NASDAQ%20Pairs%20Trade%20Strategy).

By brute-forcing over many pairs, we end up finding randomly cointegrated stocks in-sample (by pure chance), without any economic/financial rationale/edge as to why. An example of such is below:
<br>
<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/mcb.png" height="300">
<figcaption>Two completely unrelated time series showing cointegration.</figcaption>
</center>

<br>
Nonetheless, I am keeping this as a checkpoint for my learning progress. 

## Selection & Trading Phase

### Universe & Cointegration Tests

Data was daily price data from `yfinance`. I select pairs based off the bivariate Engle-Granger cointegration test on the in-sample period. This regresses one series against another to find $\beta$, creates a spread/residual series off it, then checks if the series is stationary.


This lets us select pairs which are cointegrated for the in-sample period (2018 to Jan 2021).
<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/cointegration_heatmap.png" height="950">
</center>

### Trading Phase

Once pairs are selected, we compute a spread for each pair. Then, we smooth it out, and set entry/exit thresholds. The spread is computed off a dynamic hedge ratio, evolved in online, recursive fashion via a Kalman filter (I'm sorry).

The spread is then z-scored via two rolling windows. The shorter average of the past $z_s$ days is demeaned by a longer average of the past $z_l$ days, then divided by the standard deviation of $z_l$. We enter positions when this spread is above $[z,-z]$ and exit when it crosses 0. Here was my code to simulate the trades:

```python
def simulate_trades(df, upper=0.5, lower=-0.5):
    holding, top_leg, bottom_leg, entries, exits = False, False, False, 0, 0
    for i in range(1, len(df)):
        if not holding:
            if df.z[i] >= upper:
                entry = df.y[i] - df.x[i] * df.hr[i] 
                holding, top_leg = True, True
                entries += 1
                df.iat[i, -2] = 'upper'
            elif df.z[i] <= lower: 
                entry = -(df.y[i] - df.x[i] * df.hr[i])
                holding, bottom_leg = True, True
                entries += 1
                df.iat[i, -2] = 'lower'
            else:
                entry = 0
            df.iat[i, -1] = entry
        else: 
            if top_leg == True and df.z[i] <= 0:
                exit =  -(df.y[i] - df.x[i] * df.hr[i])
                top_leg, holding = False, False
                df.iat[i, -2] = 'exit'
                exits += 1
            elif bottom_leg == True and df.z[i] >= 0:
                exit = (df.y[i] - df.x[i] * df.hr[i]) 
                bottom_leg, holding = False, False
                exits += 1
                df.iat[i, -2] = 'exit'
            else:
                exit = 0
            df.iat[i, -1] = exit 

    portfolio_val = (df.portfolio_val.cumsum()) + df.hr[0] * df.x[0]
    df.portfolio_val = portfolio_val
    rets = np.log(portfolio_val) - np.log(portfolio_val.shift(1))
    df['rets'] = rets
    df['cum_rets'] = rets.cumsum()

    return df, portfolio_val, rets
```

### Single Pair Demonstration

To illustrate, I chose an arbitrary same-sector from the backtest, INTC vs QCOM, with out-sample Sharpe of 1.467 (75th percentile) and an I.R of 0.78 (75th percentile).

Here is the price chart for the pair:

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/intl_qcom_prices.png" height="400" width="900">
</center>

The z-scored spread defines the entry and exit times:

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/intl_qcom_z.png" height="400" width="900">
</center>

Which is then reflected on the actual spread itself:

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/intl_qcom_spread.png" height="400" width="900">
</center>

Which we can track positions taken.

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/dataframes/trades.png" height="400">
</center>

 In total, we have ~4000 possible pairs, but ~400 cointegrated ones according to the test statistic and 90 of those from the same-sector.

 
## Performance

Here is an example of returns of trading an arbitarily chosen pair:
<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/intl_qcom_normrets.png" height="355">
</center>

### Sector vs No Sector Restriction Performance

I wanted to investigate if trading same-sector (GICS) pairs would perform better out-sample.
I then iterated through all selected pairs with a p-value less than the arbitrary cutoff of 0.10: ~430 pairs with no sector restriction and ~96 pairs with sector restriction.

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/sharpe.png" height="400">
</center>

The average mean and median annualized Sharpe for both sets of pairs were roughly the same ~0.60, and ~0.82, but it does look like no-sector restriction pairs have an extreme left tail of poor performers while the same-sector pairs had a floor on bad performance. 
<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/ir.png" height="400">
</center>

We can then plot cumulative (mean) returns against the benchmark:

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/cumrets_is.png" height="400">
</center>

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/cumrets_oos.png" height="400">
</center>


## Cointegration, Returns & Sector Analysis

Were same sector pairs more likely to be cointegrated?

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/pval_comparison.png" height="400">
</center>

There seems to be no difference, and I realized (as mentioned at the start of the post) that brute-force cointegration tests are just luck.



### Cointegration Out-of-Sample

To what extent does in-sample cointegration imply out-sample cointegration? Unfortunately, it seems that it is just pure chance.

<center>
<img src="{{ site.imageurl }}/QuantPairsTrade/plots/pval_is_vs_oos.png" height="500">
<figcaption>Kindly ignore the regression line.</figcaption>
</center>

This pretty much illustrates the data dredging effect. Sadly, I only realized this halfway into the project, at that point which I decided to just finish it.

## Conclusion

As mentioned several times throughout the post, blindly running a large amount of the same statistical tests on different time series will surface spurious relationships. Unless the spiders were somehow rigging the spelling bee, of course.

<!-- As seen by the example of letters in the winning word of the Scripps spelling bee being related to the number of people killed by venomous spiders.  -->

I can understand why the Kalman-filter/ADF/cointegration approach is a running meme in QuantTwit. While being unaware of it at the time of starting the project, you learn from your mistakes.

From a data science/analytics perspective, being aware of statistical biases/fallacies is an extremely important skill to have.