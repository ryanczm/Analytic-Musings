---
layout: post
title: Power Markets Notes
category: commodities
---

## Power

Power is an interesting commodity and unique on it's own relative to other commodities. Trafigura's Commodities Trading Demystified says commodity trading is about shifting commodities in time, space and form. In power, there is no form since electrons are electrons (well, ignoring fuel to power), there is no space (well assuming it flows according to a fixed grid topology/physical constraints) and almost no time (except storage). Also, auctions are used a ton.

The key, atomic unit in the power market is the transaction. This is just an agreement to involve a buyer consuming and a seller producing some varying quantity, over a coordinated period of fixed time, IN AGGREGATE, over all buyers and sellers. That's literally it. The most important thing to understand.

## Markets and Prices

Price determination (mostly) comes from the uniform price clearing auction, where bids/offers are ranked in (reverse) merit order, a uniform clearing price is assigned and each participant pays (receives) the quantity produced scaled by the uniform price paid.

Offers are ranked from low to high and bids from high to low. Offers are based off (in theory) marginal cost or fuel cost. Bids are based on demand elasticity (must have vs optional). The auction-style market can be quite confusing when people associate markets with order books.

Within that auction format, in space, the prices can be cleared at a zonal or nodal level, with the former being a more generalized/simpler version of the latter.

Markets are arranged in order of decreasing forwardness: capacity > forward > day ahead > real-time/intraday > balancing/ancillaries.

In the stack, zero marginal cost VRE sources are on the left or transferred into the demand side using residual load instead of load.

## The Stack

The stack is the model used to simulate the bidding curves (marginal cost, merit order ranking, uniform clearing etc) and demand to forecast auction prices.

## Plant View

We can divide the stack/generation mix into a tree structure. Dispatchable (thermal), storage, Nondispatchable (VRE):

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