---
layout: default
title: "RobotJames"
---


## Summary

* What is a signal - Framework of how to evaluate how 'good' a signal is.
* Log returns intuition - As it says
* Edge extraction - Formalizing what edges are and how to frame them. 4 examples.
* Data analysis for traders - TLT strategy analysis.
* Evaluating predictive signals - Are cheap stocks expensive?
* Crypto stat arb series - Comprehensive, R-code walkthrough of strategy. Do this one.

## Projects

* Small
    * TLT replication 
    * Cheap stocks expensive replication 
* Big
    * Crypto stat arb replication. Very hefty!

## What is a Signal?

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

##  Intuition of Log Returns

* Content
    * Log returns - Log return is the logged ratio of prices. Linear returns are the ratio of prices minus 1.
    * Discretization - Log returns are continuously compounding price at $r$ over infinite periods. Example shown via discretizing: $p_t=p \cdot (1+R/n)^n$
    * Symmetry - Making and losing same amount of log returns returns you to same price.
* Learnings
    * The logged ratio of two values, the log return, is the same as applying compounding it by that value many times.

## How to Make Money Trading/Basics of Edge Extraction

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
   

## Data Analysis for Traders

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

## Evaluating Predictive Features

* Content
    * Exploring a signal with no edge.
    * Exploring a really dumb factor: close prices at end of last year predict next year's return.
    * RJ does the data analysis, bucketing, etc. Finds a good effect. Signal but no edge. He wants to find why:
    * Connects to another effect (Ilmanen: volatile stocks have lower returns). Lower stocks have more volatile returns (scatter)
    * Controlling for high vol, share price factor doesn't have anything interesting to add.
* Projects
    * Using economic intuition and EDA to explore a simple factor, then discovering it is a proxy for another factor with well known edge

## Crypto Stat Arb Series

* Content
    * Ideas for Crypto Stat Arb Features - Brainstorms crypto stat arb features on crypto perpetual futures. Momo (xs), carry (xs), breakout (ts).
    * Quantifying and Combining Crypto Alphas - Creates signals, evaluates how good they are (IC, quantile plots, decay). Decides on weighting scheme.
    * A Simple, Effective Way to Manage Turnover - Backtests the strategy, performance, then uses Cephalopod's trade buffer tweet to bring down turnover rates. (⚠️ How are signals translated to weights? E.g daily carry/momentum decile rank)
    * How to Model Features as Expected Returns - Does some pretty complex stuff, but runs rolling regressions on signals to get coefficients (⚠️ How are signals translated to weights? E.g daily carry/momentum decile rank)

## Tweaking signals

* Content
    * Shows how simple tweaks on a simple signal can change PnL profile/results of a backtest
    * Assuming continuous trading/no TCs, constructs a mean reversion signal on SPY. Then crude oil futures. 
    * Then normalizes by historical vol (downgear in high vol). Then OTOH uses vol-normalised returns to compute signal.
* Learnings
    * Small signal tweaks can change the profile alot.
* Project
    * Short project replicating his signal tweaking.

## Option Pricing for Degenerate Gamblers

* Content
    * RJ visualises the stock price over time. Current time $t$. Option expiry time $t+n$ (vertical line). Horizontal mark on the strike price.
    * Option prices reflect a view of _probability distribution of prices at expiry_. In particular, a subset of the purple histogram, the yellow histogram.
    * _Fair value of a bet_ is the sum of probability of outcomes * payoff if it happens. Price you don't expect to make money if you're long or short.
    * Visualises delta. Visualises theta. Visualises vega.
    * The edge in options is ability to estimate the purple probability histogram, more accurately, than the aggregate markets.
* Learnings
    * Key intuition is that we can have a view on a range of outcomes with no fixed certainty. It's about estimating over a range of possible futures, not one.
