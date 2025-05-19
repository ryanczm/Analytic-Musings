---
layout: post
title: Flirting with Models
category: quant
---

## Overview

Keeping some notes from Corey Hoffstein's Flirting with Models here. I'm mostly interested in episodes about CTA.

## Doug Greenig (Florin Court) - At the Frontier of Trend Following

Florin Court's strategy is to trade a large number of alternative markets that are as uncorrelated as possible. Doug Greenig wants the firm to be the best alternative markets CTA. For me this was the best podcast of all of them given I am interested in CTA/systematic commodities.

Why do Trends Exist?

* "One of the reasons trend works is that when narratives change, there tends to be a burst of volatility. And it happens on many scales and markets - that burst of volatility around narrative shifts and trend reversals is pretty key to how trend following works".
* The most important statement in the podcast.
* Of course, trends exist due to fundamental changes, but narrative changes as well. 

CTAs Return Profile

* Not the highest Sharpe ratio but a nice positive skew when dislocations occur (CTAs profit off tail events with sudden trends). Challenge is to boost Sharpe.
* Normal CTAs trade in general about 100 futures and FX forward markets.

Alternative Markets 

* Florin Court trades 500 markets. The idea is to expand into 'alternative' markets that are orthogonal or uncorrelated as possible  to developed markets.
* Alternative Markets are uncorrelated to general macro themes, like Fed monetary policy and Chinese economy.
* Diversify into international commodities/macro markets - 'French electricity, Colombian interest rates, Turkish cross currency swaps, Californian carbon emissions, South African wheat, FFAs, Chinese steel rebar', etc.
* Use their uncorrelated properties to boost diversification and Sharpe.
* Liquidity - There is a misconception alternative markets are illiquid. Doug says this is not true. It's just that traditional funds don't have access, but the action is going on. For exammple, the Chinese domestic commodity markets.

Trend Signal

* Medium to long term - a quarterly trend is ideal. 

Operational Edge, Onboarding Alternative Markets, OTC Transactions

* SystematicLS talked about operational edge. Doug says the main business is in expanding into these alternative markets and assessing feasibility. Talking to brokers, dealers, markets that trade bilateral OTC and not via electronic exchange.
* Going into the a new market, they start small in size, gather data on their executed trades, like bid-offer spread width, etc etc to get a feel of liquidity, and they decide whether to continue or not.
* Execution is not always algorithmic/electronic in these markets, they use execution traders to do manual. Interesting!

Synthetics

* If one instrument is not amenable to trend, they actually construct synthetic futures from a combination of assets and trade that. Examples not disclosed. But cool.

Price Properties of Alternative Markets

* This was the part I found most interesting and applicable to me. Doug says alternative markets are less choppy. The less speculative traders there are, the less choppy, the more trend and more amenable to trend following. Of course this is at the expense of liquidity.

Portfolio Construction and Capacity

* They run 3bn capacity. Too big and you become not agile. You have to allocate more into developed markets with the liquidity. Obviously too much size in a small alternatives market causes transaction costs to eat the alpha.
* How do they allocate risk in a portfolio of 500 forward/futures markets?
* For portfolio construction, it was quite vague. They use a tree structure to allocate risk such that I guess each node in the tree at that level has equal amount of historical vol/risk. And it takes into account correlations systematically too, like butane and propane being correlated. Interesting. I guess it involves classifying the various assets into a hierarchical format, taking into account their historical vol and correlation to other asset, and dynamically adjust. Sounds like MLDPs HRP.

Summary

* These guys are just harvesting trend premia off obscure forward curves all over the world. Cool.
* I like the connection to SystematicLS talk of edges: an operational edge. Clearly these guys have op alpha in getting access into new markets and onboarding them.
* As the number of bets you have increases, you can't follow the fundamentals of each market carefully. Constrast to my time at Alpha Cygni where you had a tight range of commodity futures and you could track fundamentals for each set. At 500, you probably look at their beta to macro/geopol as a whole rather than individual. 
* If the average CTA has 100 futures/forwards and Florian Court does 500, that is indeed impressive.
  

## Nicolas Mirjolet (Quantica) - A Multivariate Approach to Trend Following

The focus of the podcast was on the unique style of trend following Quantica employs called multivariate trend following, which involves constructing trend signals using not just the asset chosen but of trend of other assets. Of course, cross-correlation comes into play. A concrete example is not given but Quantica Managed Futures fund trades 100 instruments and does medium to long term trend (lookback window of a quarter, same as Florin Court), but with the magic multivariate trend following sauce. Quantica also publishes research papers on CTA. Might be worth a look for a project replication.

Multivariate Trend Following

Trend Following Performance and Statistical Risk Factors


## William Gebhardt (10Dynamics) - Replicating Discretionary Commodity Trading Systematically

The focus of this podcast was on 10Dynamics unique approach to signal construction. In particular, they add a bunch of binary signals together for each instrument.

Adding Binary Signals