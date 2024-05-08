---
layout: default
title: Flirting with Models - Macrocephalopod (Apr 24)

---

<!-- omit in toc -->
## Background

Macrocephalopod's interview on Flirting with Models podcast by Corey Hoffstein. [S6E5](https://www.youtube.com/watch?v=5qo5gxmGEt8&t=1502s&ab_channel=FlirtingwithModels), 31-May-2023. 

[❓] represents something I need to investigate and learn about.

## On Background and Past Strats

* Strategies in the vein of style premia [❓]
* Started in 2010s implementing low frequency strats. CTA trend following. Carry strategies. Macro momentum (surprises). Trying to build LS portfolios around these ideas.
* They worked because of a genuine risk premia, a carry strategy, that makes you subject to big left tail crashes (particularly in FX/rates) + inefficiency edge [❓]
* Inefficiency came about due to difficulty of accessing data / markets. No nice wrappers for non-institutional players to access. Now, if you want access a momentum strat, you buy an ETF that wrapped up a momentum strategy. [❓]
* If you carry: long currencies with high interest and short currencies with low interest made a Sharpe of 1.5-2 which ended in 2008.
* Why have the parallel strats decayed since yet not in equities?
  
---
* [❓] CTA trend following 
* [❓] Macro momentum
* [❓] Carry across diff asset classes
* [❓] Left tail property of carry esp FX/rates
* [❓] Properties of equity markets that led the alpha not decay

## On Role of Backtesting 


* Research
  * Backtest will always be optimistic. In live trading, will be worse.
  * Backtest is not a research tool. Research is what you do before the backtest. Backtest validates the idea.
  * Research is looking at data and building models and understanding effects BEFORE thinking about simulating.
  * You can go a long way with building some features, doing regression and finding corr of pred with forward returns. Million ways to mess things up doing a linear regression.
  * Things you can do before a backtest: how to transform features, which features useful, how to combine multiple features into final alpha, what are sensible position limits or vol forecasts or market beta exposure.
* Backtest
  * Take alphas, vol forecast, risk model, trading constraints > put into a backtest to see what kind of performance can I expect. [❓]
  * _Compare two diff variants of same strat_, two diff ways of constructing your alpha. Same set of features, two diff methods, delta between those two backtests is something might be meaningful. 
  * _Reconciliation vs live trading_: at the end of day, you run your simulated trading backtest vs your live performance and say how well does my backtest reflect reality and think about the differences. 

---
* [❓] Vol forecasts as input to a backtest (why)
* [❓] Market beta exposure as input to a backtest (why)

## On Factor Models