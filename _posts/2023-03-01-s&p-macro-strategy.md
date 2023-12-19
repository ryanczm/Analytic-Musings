---
layout: post
title: An S&P Macro Rebalancing Strategy
category: quant
---

In this post, I come up with a strategy to beat the benchmark of long S&P by constructing a long-short portfolio consisting of the S&P 500, Gold, and US10Y treasury bonds. This post was adapted from the take-home project from AVM Capital, a quant macro hedge fund.
<!--more-->The [repo](https://github.com/ryanczm/qr/blob/master/economic_regime_avm.ipynb) to my project is on Github.


The data given to me was daily close of S&P, quarterly close of Gold, daily yields of US10Y bonds, quarterly GDP YoY change figures and monthly CPI YoY change figures, from 1970 to 2020. 

The in-sample period is from 1970 to 2012 and the out-sample period is from 2013-2020. I then evaluate the out-sample performance based on the rolling Sharpe, rolling information ratio and tracking error.

<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_macro_indicators.png">
<figcaption>Henri Poincaré 1854-1912</figcaption>
</center>

I was to rebalance the portfolio quarterly based on the current regime the quarter was in, determined by whether growth or inflation was rising/falling from the previous quarter. 


## Regimes

First, we create quarterly log returns:

```python
def create_returns(prices, yields):

    prices['SR'] = np.log(prices['S&P 500']) - np.log(prices['S&P 500'].shift(1))
    prices['GR'] = np.log(prices['Gold']) - np.log(prices['Gold'].shift(1))
    yields['YR'] = np.log(yields['US10Y']) - np.log(yields['US10Y'].shift(1))
    
    rets = pd.merge(prices, yields['YR'], left_index=True, right_index=True)
    return rets.resample('Q').sum()
    
df = create_returns(prices, yields)
```


Next, given the CPI and GDP data, we can classify any period in this time horizon into whether it has rising or falling growth or inflation relative to the previous year. We categorize an indicator as rising if it is greater than the previous quarter and falling if it is lesser than the previous quarters values. Thus, this gives us very simplistic categories:

1. Bearish - Falling growth, falling inflation
2. Stagflation - Falling growth, rising inflation
3. Bullish - Rising growth, falling inflation
4. Overheating - Rising growth, rising inflation

```python
def assign_regimes(df, macro):
    regimes = pd.DataFrame(index=macro.index)
    regimes['g'] = np.where(macro['GDP YOY'] > (macro['GDP YOY'].shift(1)), 1, 0)
    regimes['i'] = np.where(macro['CPI YOY'] > (macro['CPI YOY'].shift(1)), 1, 0)
    regimes['r'] = (regimes.g.astype(str) + regimes.i.astype(str))
    regimes['r'] = regimes.r.apply(lambda x: int(x,2)
    return df.merge(regimes.r,left_index=True, right_index=True,how='inner')

def split_rets(rets, date):
    train, test = rets[rets.index < date], rets[rets.index >= date]
    return train, test

rets = assign_regimes(df, macro)
train, test = split_rets(rets, '2012-01-01')
```


## Signal

The strategy emits 4 fixed alphas (one for each regime) which are normalized so their absolute values sum to unity for long/short portfolio weights to take in each regime. The alpha vectors are naively created from normalizing the Sharpe ratio of the returns during each regime (for all time periods) on the training data.

```python
def sharpe(return_series, n, rf):
    mean = (return_series.mean() * n) - rf
    sigma = return_series.std() * np.sqrt(n) 
    return mean/sigma

def create_weights(rets):
    alphas = rets.groupby('regime')[['SR','GR','YR']].apply(sharpe,n=4,rf=-0.03) 
    weights = alphas.div(abs(alphas).sum(axis=1), axis=0)
    return alphas, weights

alphas, weights = create_weights(train)
```

This is a simple method of creating an alpha that uses a summary statistic (Sharpe) of returns for each regime. However, the returns of each regime are not contiguous, because the regimes switch every quarter or so. An alternative method would be MVO, but will stick with the former. The barchart shows the portfolio allocation in each regime.

<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_weights.png">
<figcaption>Henri Poincaré 1854-1912</figcaption>
</center>

<h2>Performance</h2>
<p>Once we compute the return series for our strategy, we can compare the performance to long S&P only. Since the signal is fixed (4 alpha vectors), there is no quantile analysis or other diagnostics.</p>
<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_is_cumrets.png">
</center>

<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_oos_cumrets.png">
</center>


<p>Unfortunately, the strategy does overfit. While overall cumulative returns for both in-sample and out-sample period does beat the S&P, the tracking error plots show many instances where it had lesser returns than the market index.</p>

<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_tracking_error.png">
</center>
<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_tracking_error_histogram.png">
</center>
<p>Looking at the out-sample 1Y rolling information ratio and Sharpe ratio, we see more evidence of the strategy failing to perform in the Covid era and beyond, showing a decrease in 2020 onwards with a slight positive upturn at the end of the out-sample period (mid 2022). Thus, it seems like a case of overfitting.</p>

<center>
<img src="{{ site.imageurl }}/SPMacro/plots/graph_oos_ir.png">

<img src="{{ site.imageurl }}/SPMacro/plots/graph_oos_sharpe.png">
</center>

<h2>Conclusion</h2>
<p>Unfortunately, the strategy seems to be very naive and underperforms with negative rolling IR and Sharpe towards the end of the out-sample period due to overfitting. Also, the method of classifying regimes is simple and naive.</p>
<p>However, this was a good exercise in getting my hands dirty with some financial data and playing around. Furthermore, it was made aware to me that my strategy had lookahead bias, as the quarterly GDP/CPI YoY figures were usually released after a quarter had already started. Thus, I was prompted that possibly lookahead macro indicators could be used as proxies of inflation and growth.</p>
