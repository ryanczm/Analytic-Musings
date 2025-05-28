---
layout: post
title: Power Markets Notes
category: commodities
---

## Power

Power is an interesting commodity and unique on it's own relative to other commodities. Trafigura's Commodities Trading Demystified says commodity trading is about shifting commodities in time, space and form. In power, there is no form since electrons are electrons (well, ignoring fuel to power), there is some space (well assuming it flows according to a fixed grid topology/physical constraints) and almost no time (except storage). It is a commodity where microeconomics and auction theory dominates. 

The key, atomic unit in the power market is the transaction. This is just an agreement to involve a buyer consuming and a seller producing some varying quantity, over a coordinated period of fixed time, IN AGGREGATE, over all buyers and sellers. That's literally it. The most important thing to understand.


## Market Alphas

These alphas concern themself more with market participant behavior and (micro)economics. 

* Geography - Understanding geography of various country grids.
* Portfolio/Deals - Different portfolios and deals (PPA/offtakes/etc.) 
* Auctions - Understanding auction formats, clearing mechanisms.
* Supply Side - Understanding power plant microeconomics & bidding behavior in auction markets.
* Load Side  - Understanding the breakdown of load in terms of consumer behavior

## Technical Alphas

These alphas are related to basic understanding of introductory EE concepts at a qualitative level aka a 'YouTube' level. Some good channels: [Prof Mad](https://www.youtube.com/@Profmad), [Electrician U](https://www.youtube.com/watch?v=GCiVNAwErnQ)

* Electron Intuition - Drude model, water model
* Elementary circuits & Loads - Resistors, capacitors, inductors, parallel/series. 
* Load Technicals - Load adjustment (cross section, time series), load types
* Basic circuits - Inverters, rectifiers
* AC vs DC - Impedance, phasors, projection, power triangle, 3P AC, VAR
* Circuit Laws - KVL/KCL/Ohm/Power
* Flows Intuition - PTDF/linearization/superposition theorem

## Blended Alphas

These are alphas that combine the two - requiring knowledge from market alphas and technical alphas.

* Supply Side - Understanding types of plants, their mechanics of power production.
* Demand Side - Understanding the various types of loads on a technical level and an econonomic level.
* Grid Side - Grid interconnections, congestion, SCED/nodal pricing (e.g. in a 3 bus system)

## Power Markets vs Commodity Futures Markets

It is tempting to associate power markets (the most quanty commodity) with commodity futures markets. This is a mistake! 

Financial markets (e.g. what I have some experience in - commodity futures) are very noisy and you need lots of statistical rigour to be able to confirm/disprove hypotheses about markets. For example, a certain fundamental signal of flow of supply/demand might a certain effect on return in a certain scenario on a certain horizon, etc. However, you need to orthogonalize out against other effects.

Power markets on the other hand, are alot less statistically noisy and you don't need the same level of statistical machinery as generators/retailers mostly behave in very predictable fashion. Instead, you need to put yourself in the perspective of the market participant in question (e.g. the generator or retailers) and understand how they would behave.

The power market is then the aggregation/ensemble/dynamical system of these predictable physical participants doing very simple, common sense things to maximize their revenue. For example - you look at generation data, transmission data, price data - and you deduce what common sense, revenue maximizing behavior is going on.

So the key alpha - is to put yourself in their shoes and see what 'common sense behavior' is being done.

## Markets and Prices

It turns out that there are two very good textbooks on this. Stoft's *Power System Economics* and Kirschen's *Fundamentals of Power System Economics*.

Price determination (mostly) comes from the uniform price clearing auction, merit order & marginal cost bidding. The cI don't claim to understand US zonal/SCED style markets.



## Stack

The stack is the model used to simulate the bidding curves (marginal cost, merit order ranking, uniform clearing etc) and demand to forecast auction prices. 

## Dispatch View

Baseload gets filled by nukes first, then RoR hydro (depending on the aggregated flow profile), then seasonal coal, then peaking gas/oil, then VRE (solar/wind), and storage is layered on top to shift energy in time at different frequencies (think of pumped hydro as LF/MF shifting and batteries as MF/HF shifting).

## Portfolios

Portfolios (generation, retail, prop, etc) refer to how power market participants earn money. The main principle is to remember any transaction must involve a buyer consuming and a seller producing the same quantity, over a coordinated period of fixed time, but many middlemen/intermediaries can be involved, such that the true physical parties (gen and load) may not have any idea who their counterparties are!

With this principle, you don't need to be capable of gen or load by yourself, if you are a middleman, to trade physical power. In that sense, prop trading in power is alot like market making.

## Logistics

NG and power are unique in that the logistics burden are mostly on the grid/pipeline network operators, not the physical trader.