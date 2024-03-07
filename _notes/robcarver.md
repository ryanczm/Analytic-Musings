---
layout: default
title: "Rob Carver"
---
# Rob Carver

## Projects

* Systematic futures strategy
    * From his book, select a weighted portfolio of diversified futures. Do something and backtest. 


### Summary

* __Basic Directional Strategies__
    * Basic trends ✅
    * Position sizing with risk ✅
    * Weighting for multiple instruments
    * Multiple trend following rules
    * Basic Carry
* __Advanced Trend Following & Carry Strategies__
* __Advanced Directional Strategies__
* __Relative Value Strategies__
* __Tactics__


### Basic Directional Strategies

* _Basic Trends, Position Sizing_
    * Back-adjustment
        * Stitching together continuous futures price series creates gap on roll dates. Use back-adjustment (Panama canal method) to create continuous series, but trade on original series.
    * Risk of Single Contract
        * $ P \cdot M \cdot \sigma_{\%}=\sigma_{p}$. Dollar risk is the notional times percentage risk.
        * Carver defines it as $\sigma_{\%}=16\cdot(0.3(10Y \space avg)+0.7(\sigma_{\%}))$
        * The larger weight is on a 10Y average of annualized risk. The shorter weight is a EWMA32 of returns. Is the 10Y estimate from monthly EWMA32s of past 10 years, averaged out? or an SMA?
    * Trend Strength
        * $R = \frac{T}{\sigma_{p}}= \frac{T}{P \times \sigma_{\%} \div 16}$. The raw forecast is the trend divided by the daily dollar risk
        * $S = R \times 10 \times scalar$ where the scaled forecast is multiplied by 10 and the forecast scalar (absolute value of raw forecast). Then, $\hat{S} = cap(S)$ Carver caps the scaled forecast at $\pm 20$.
    * Position Scaling
        * $N=\frac{\hat{S} \times C \times IDM \times w \times \tau}{10 \times M \times P \times \sigma_{\%}}$. The position is the capped forecast times capital times IDM times weight times risk target divided by 10 times notional times percentage risk.
* _Multiple Instruments_
    * Portfolio types
        * Risk parity - Equity risk premium (beta) and bond risk premium (duration). In this case, 50-50 in S&P micro future & US 10Y bond future.
        * All weather - 25% S&P 500 micro future, 25% bonds (half 10Y half 5Y), 25% commods (half WTI futures, half corn futures), 25% gold micro futures.
        * Jumbo - Everything
    * IDM
        * In in-sample period window, calculate annualized portfolio risk from each instrument. $IDM=\tau / \sigma$. This is how much to scale signal for each asset by.
    * Weight Allocation
        * Top down/handcrafting method means equal weight by asset class, group, instrument. Hierarchical. Requires a classification scheme, a dendrogram of choice.
    * Instrument Selection
        * Pretty complex, but once you have weights, you need to select instruments for each category. Skipped.
* _Multiple Trend Following Rules_
    * Can diversify across filters/rules: produce a single forecast combined from different filters. Combined capped forecasts via _weights_. Could be pre-cost SR.
    * Carver demonstrates dendrogram/handcrafting: by style, then rule, then variation.
    * Introduces _FDM_, similar to _IDM_.
* _Basic Carry_
    * Carry
        * _Carry_ is excess returns unrelated spot price changes from owning asset. 
        * Direct carry: Dividends (equities), yields (bonds), deposit/borrowing rate (FX), commods carry (borrowing cost, storage cost, convenience yield).
        * For futures, the carry effect will be _naturally incorporated_ into the forward prices/term structure due to no-arbitrage theory. 
        * E.g for Eurodollar interest rate futures, front month is at 99 but next contract is at 98. So can roll, earning the carry. 
        * Positive carry assets have higher risk (from holding them), negative skew. Earn carry as _insurance_. E.g EM currency vs dollar.
    * Calculating Carry
        * Raw carry is differential between the near contract and next nearest contract. Annualize this, then scale by instrument risk to get _risk-adjusted carry_.
        * Carry forecast = annualized raw carry / risk forecast x 16.
        * How to trade carry (?), take the carry forecast signal (EWMA), if high, you long near month and short next nearest future to earn carry? 
