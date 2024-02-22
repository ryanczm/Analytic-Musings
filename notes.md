---
layout: page
title: Notes
order: 4
---

## Scraping QuantTwit for Wisdom



---
### Level 1: RJ/EPQ

#### Summary

* What is a signal - Framework of how to evaluate how 'good' a signal is.
* Log returns intuition - As it says
* Edge extraction - Formalizing what edges are and how to frame them. 4 examples.
* Data analysis for traders - TLT strategy analysis.
* Evaluating predictive signals - Are cheap stocks expensive?
* Crypto stat arb series - Comprehensive, R-code walkthrough of strategy. Do this one.

#### Projects

* Small
    * TLT replication 
    * Cheap stocks expensive replication 
    * Tweaking signals 
* Big
    * Crypto stat arb replication. Very hefty!  

#### What is a Signal?

* Content
    * Signal - A _signal_ is something that predicts the price of a thing at $t+n$, at now $t$, over a forecast horizon, using/function of information. __(signal chart)__
    * Information - New info appears as time passes. The older information is, the less valuable __(information decay chart)__
    * How Good Is It - Scatterplot. Look at correlations. Use _reduction_, say, the mean value per quantile. This is scatterplots are too messy/crowded. __(reduction chart)__
    * Jumpiness - Too _jumpy_ leads to high turnover.  So you _smooth_ it. Tradeoff between jumpiness and goodness __(smoothed signal chart)__
    * Decay - For each time period, calculate the goodness (correlation), plot against forecast horizon. You want slower decay __(correlation vs horizon chart)__
* Learnings
    * We've learned what a signal is (not how to construct it).
    * A good signal is not jumpy, predicts well, decays slowly (predict well for longer time periods).
* Project/Ideation
    * Can we do a project where we construct signals and evaluate them with these techniques? Look at info, info decay, goodness of fit, jumpiness, time decay.

####  Intuition of Log Returns

* Content
    * Log returns - Log return is the logged ratio of prices. Linear returns are the ratio of prices minus 1.
    * Discretization - Log returns are continuously compounding price at $r$ over infinite periods. Example shown via discretizing: $p_t=p \cdot (1+R/n)^n$
    * Symmetry - Making and losing same amount of log returns returns you to same price.
* Learnings
    * The logged ratio of two values, the log return, is the same as applying compounding it by that value many times.

#### How to Make Money Trading/Basics of Edge Extraction

* Content
    * Trading
        * Trading - Buy things cheaper than they should be, sell at richer than they should be. Identify when mispriced. 
        * Usefulness - Doing useful things that suck. Why are you getting paid?
        * Edge - Reason/rationale for why your strategy can work in terms of market participants behavior. Can have edge with no signal (simple), signal with no edge (no clear reason). What if you discover a really good signal but have no idea why it works (rationale)?
    * Laws of Asset Pricing
        * Stuff that's the same should have the same price
        * Technical supply/demand imbalances distort prices
        * Information is incorporated in current price (no edge in forecasting)
    * Rationales
        * Who is trading at bad prices? 
        * Why is the trading useful (add value, why are you getting paid)?
        * What sucks about it?
    * Examples
        * Latency arb
        * FTX leverage token (systematic rebal)
        * Turn of month window dress (TLT)
        * STIR curve
* Learnings
    * RJ's edges are based on exploiting systematic behavior of market participants.
    * Post on idea-first strategy: based on some hypothesis, come up with a strategy.
    * To identify edge, need understanding of how market rules work.
   

#### Data Analysis for Traders

* Content
    * Framework
        * Test small little hypotheses (your pet theories)
        * Test out what you would see if your hypothesis were wrong.
    * Example
        * Walks through the TLT seasonality example. Returns by day of month.
        * Metric of returns of last 5 days of month minus returns of first 5 days of month, per month.
        * Investigates: across sub periods (year, years), effects in other assets, absolute returns (not risk premium)
* Learnings
    * Shows analysis for idea-first strategies: how to test out a hypothesis.
* Project
    * Replicating the TLT analysis exercise.

#### Evaluating Predictive Features

* Content
    * Exploring a signal with no edge.
    * Exploring a really dumb factor: close prices at end of last year predict next year's return.
    * RJ does the data analysis, bucketing, etc. Finds a good effect. Signal but no edge. He wants to find why:
    * Connects to another effect (Ilmanen: volatile stocks have lower returns). Lower stocks have more volatile returns (scatter)
    * Controlling for high vol, share price factor doesn't have anything interesting to add.
* Projects
    * Using economic intuition and EDA to explore a simple factor, then discovering it is a proxy for another factor with well known edge

#### Crypto Stat Arb Series

* Content
    * Ideas for Crypto Stat Arb Features - Brainstorms crypto stat arb features on crypto perpetual futures. Momo (xs), carry (xs), breakout (ts).
    * Quantifying and Combining Crypto Alphas - Creates signals, evaluates how good they are (IC, quantile plots, decay). Decides on weighting scheme.
    * A Simple, Effective Way to Manage Turnover - Backtests the strategy, performance, then uses Cephalopod's trade buffer tweet to bring down turnover rates. (⚠️ How are signals translated to weights? E.g daily carry/momentum decile rank)
    * How to Model Features as Expected Returns - Does some pretty complex stuff, but runs rolling regressions on signals to get coefficients (⚠️ How are signals translated to weights? E.g daily carry/momentum decile rank)

#### Tweaking signals

* Content
    * Shows how simple tweaks on a simple signal can change PnL profile/results of a backtest
    * Assuming continuous trading/no TCs, constructs a mean reversion signal on SPY. Then crude oil futures. 
    * Then normalizes by historical vol (downgear in high vol). Then OTOH uses vol-normalised returns to compute signal.
* Learnings
    * Small signal tweaks can change the profile alot.
* Project
    * Short project replicating his signal tweaking.

#### Scaling by Vol

* Content
    * Compares BTC returns to hypothetical, 20% vol strat. BTC is much better. 
    * However, scaled by vol, both the same (vol ratio). Then shows a plot of leveraged 6x strat 

---
### Level 2: MC/CH

#### Projects

* Small
    * Zillow adverse selection replication
    * Trade buffer derivation
    * Non overlapping period replication
    * BTC/SPX rolling correlation replication
    * Equity factor model linear algebra derivation
* Big
    * RobotJames' crypto stat arb

#### Discrete vs Continuous Trading

* Content
    * Rather than use signal as entry/exits, discretizing throws away information. 
    * E.g waiting for a MACD cross then ascribing special value to that event, track diff between two MAs as signal. 
    * At rebal freq, take change in signal, put it in position function, then adjust position accordingly.
* Learnings
    * Think of signals as rebalancing rather than discrete entry/exit.

#### Trading Stuff Amateurs Care but Practitioners Disregard

* Content
    * Stationarity tests, cointegration, vol forecasting, exit signals, stop losses.
    * First two are pointless because any such relationship will not last in the future and is just chance.
    * Last two are pointless because those problems arising from discrete trading, not continuous.

#### Equity Factor Model Megathread



#### Trend Following Breakdown

* Content
    * Trends exist. No need to know 'why'. Can be exploited systematically. Components of a trend-following strategy:
    * Universe selection
        * Trade as much _uncorrelated_ markets as you can. E.g if you trade S&P and Nasdaq futures, DJIA no benefit. But something like natural gas futures will help.
        * Trend followers trade futures due to leverage. Something to do with MTM? Why futures? ⚠️
    * Signals
        * Most common are MAC variations. E.g fast MA of (log) price minus slow MA normalized by vol. 
    * Position
        * Passed into fn mapping signal to target risk allocation for that market. E.g a sigmoid kernel. To cap out position as trend gets stronger. Add bias to fn for markets that drift up on average.
    * Sizing
        * Scale position inversely to market-specific vol forecast, so each market contributes equally to risk. Control.
    * Sector-specific weights
        * ⚠️
* Learnings
    * Professional breakdown of trend-following in futures strategies. Across diff sectors. Careful sizing, simple signal.

#### Trade Buffer

* Content
    * Once you have decent alpha the skill in systematic strategy is to reduce turnover.
    * Simple heuristic (no optimimization) is a trade buffer. Strategy outputs target position $x$. Current is $y$
    * Interval of width $2b$ around $x$. If $y$ outside $b$, trade till $y=x\pm b$.
    * If $y$ is within $x\pm2b$, do nothing. This way, you only trade on large moves in position from current. Larger $b$ = less turnover.   
    * To pick it, optimize in backtest (max Sharpe), or target a particular turnover level.
    * RJ applies this in his crypto stat arb post on RobotWealth.
    * Mathematical derivation ⚠️
* Learnings
    * A simple non-optimization rule to reduce turnover given enough skill in alpha.

#### Portfolio Optimization: Non-Experts vs Practitioners

* Content
    * Naive view of portfolio optimization - vector of alphas, cov matrix, costs, constraints, current pos, maximize objective. This gives bad results!
    * Alternative - Use a heuristic solution that works. Start with signal vector $s$. Normalize it to weights. Apply buffering.
* Learnings
    * Focus on simple solutions before complex solutions.

#### Zillow Adverse Selection Thread

* Content
    * Even if Zillow's model was more accurate than local agent valuations, local agents win on average because they choose which houses to sell
    * Generates dataset of uniformly distributed housing prices (true values), then adds noise. Zillow is 5% noise, agents are 10% noise. Both estimates are unbiased but Zillow has less variance. 
    * Agents sell, Zillow buys then sells. Plot on scatter: axes with agent and zillow valuation.
    * Agent sells to Zillow if Z valuation is 20% higher than agent. Agents only choose to transact this. They then sell houses at true value. 
    * Plots PnL and negatively skewed due to adverse selection.
* Learnings
    * Only transaction where Zillow forecasts wrongly will occur because agent will only choose to sell when Z valuation is much higher.
    * Very interesting thread on adverse selection in transaction. Works because agents sit on houses waiting for offers, they have choice to transact or not.

#### Non-Overlapping Periods

* Content
    * Scatterplot of cyclically adjusted P/E ratio and 10Y forward returns. Shows R2 of 0.65, super strong correlation.
    * Ceph says due to overlapping periods of returns, generates fake statistical significance. S&P 10Y returns, non-overlapping, since 1950 = 7 dots. Overlapping = 100s of dots.
    * Easy way to trick yourself into statistical significance. The $n$ is fake.
* Learnings
    * Data hygiene in how you analyze scatterplots. Don't overlap returns too much!

#### Strategy Naming/Dichotomy

* Content
    * Quantitative/systematic/automating trading are not same thing.
    * Automated = no human making decision.
    * Systematic = how high level decisions are made.
    * Quantitative  = technical model involved. Not MACs!
    * Trend following is systematic, not quantitative. Can have quantitative discretionary equity L/S, where lots of quant modelling.
* Learnings
    * These terms have nuance! 

#### BTC/SPX Rolling Correlation

* Content
    * Plots rolling corr between BTC/SPX from 2018-2023. Daily correlation on 5 minute bars (1 day = 1 dot). Then plots MA of daily corr.
    * In 2022, BTC was very coupled to stocks. Now showing signs of decoupling again.
* Learnings
    * Correlation between two time series, rolling is always better! More granular.
