---
layout: default
title: "Macrocephalopod"
---

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
