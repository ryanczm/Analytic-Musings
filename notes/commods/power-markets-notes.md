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

These alphas are related to basic understanding of introductory EE concepts at a qualitative level.

* Electron Intuition - Drude model
* Basic circuits & Loads - Resistors, capacitors, inductors, parallel/series, load adjustment (XS vs TS), load type
* AC vs DC - Power triangle/factor, 3P AC, VAR
* Circuit Laws - KVL/KCL/Ohm/Power
* Flows Intuition - PTDF/linearization/superposition theorem

## Blended Alphas

These are alphas that combine the two - requiring knowledge from market alphas and technical alphas.

* Supply Side - Understanding types of plants, their mechanics of power production.
* Demand Side - Understanding the various types of loads on a technical level and an econonomic level.
* Both - Understanding SCED/nodal pricing - e.g. in a canonical 3 bus system.

## Markets and Prices

Price determination (mostly) comes from the uniform price clearing auction, merit order & marginal cost bidding. I don't claim to understand US zonal/SCED style markets.

It turns out that majority of power markets are just creative auctions, with parameters varying on the product sold, the stack order, the time frame, the participants on the demand/supply side. For example, I find the 2-stage balancing/ancillary service auction in European power markets quite interesting.

## Stack Order

The stack is the model used to simulate the bidding curves (marginal cost, merit order ranking, uniform clearing etc) and demand to forecast auction prices. We can divide the stack/generation mix into a tree structure. Dispatchable (thermal), storage, Nondispatchable (VRE):

* Dispatchable 
  * Thermal (Nukes > coal > oil/gas)
* Non-Dispatchable
  * Renewables (Solar, wind, hydro)
* Storage
  * Electrical (BESS)
  * Mechanical (Pumped hydro)

Dispatchables are thermal plants. They run on the same principle: convert fuel to heat and heat to power in a controllable fashion. There is a tradeoff between baseload and peaking (due to startup/shutdown costs and marginal fuel costs) in ascending order: nukes > coal > gas/oil. It is important to remember they work on the same underlying mechanism but under different parameters according to how they convert their differnet fuels to heat to spin the turbine.

Non-dispatchables (VRE) are solar, wind, hydro (run-of-river). These have zero marginal cost but different variability profiles according to the aspect of weather they use as fuel (wind, solar radiation, river flows). There is no heat involved.

Storage bridges the gap between the dispatchables and non-dispatchables, it shifts energy/power in time. It is important to remember BESS vs PH have very different shifting profiles and work on different mechanisms. For example, for PH, head, penstock width, resevoir capacity, RTE due to friction, etc etc.

## Dispatch View

Baseload gets filled by nukes first, then RoR hydro (depending on the aggregated flow profile), then seasonal coal, then peaking gas/oil, then VRE (solar/wind), and storage is layered on top to shift energy in time at different frequencies (think of pumped hydro as LF/MF shifting and batteries as MF/HF shifting).

## Portfolios

Portfolios (generation, retail, prop, etc) refer to how power market participants earn money. The main principle is to remember any transaction must involve a buyer consuming and a seller producing the same quantity, over a coordinated period of fixed time, but many middlemen/intermediaries can be involved, such that the true physical parties (gen and load) may not have any idea who their counterparties are!

With this principle, you don't need to be capable of gen or load by yourself, if you are a middleman, to trade physical power. In that sense, prop trading in power is alot like market making.

## Logistics

NG and power are unique in that the logistics burden are mostly on the grid/pipeline network operators, not the physical trader.