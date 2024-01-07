---
layout: post
title: "Paper Replication: Crude Oil Contango Arbitrage and the Floating Storage Decision (Regli & Adland)"
category: commodities

---
In this post, I analyze  Regli's - Crude Oil Contango Arbitrage and the Floating Storage Decision 2019 paper, which investigates and models historical floating storage arbitrage opportunities (GFC, 2008 and the Oil Glut, 2015) when the crude oil curve is in contango.
<!--more-->
<center>
<img src="{{ site.imageurl }}/FloatingStorage/profits.png" style="width:55%;"/>
<!-- <figcaption>Required reading.</figcaption> -->
</center>

<!-- Given my background as an Energy Analyst, it was quite surprising that my time there did not equip me with skills to understand commodity markets. -->
As per my previous post, due to time constraints (did this in 2 days) I did not _replicate_ the paper _per se_, e.g getting the data, coding and evaluating the results (soon!). Is is difficult for me to get access to commodity and freight data.


The paper is divided into several parts: it explains formulas a floating storage trade & the no-arbitrage condition, then simulates the profitability of floating storage from 2006-2018, and analyses excess profits over a FFA-hedged time-charter strategy (using the vessel for transport instead of storage).

# The Floating Storage Trade

A floating storage trade is a "cash-and-carry" trade to exploit the contango structure of the forward curve of crude. Intuitively, this makes sense: if contango is caused by oversupply of stocks in the short term, the market incentivizes participants to 'correct' the oversupply by storing stocks for future consumption.

The authors state a similar equation to the past paper, with continuous compounding for the profit:

$$\pi = (F_{t}(1 - TRC) - e^{r(T-t)}(S_{t}(1 + TRC) + INS))w - e^{r(T-t)}(TC_{t}(T - t) + MGO_{t} + PC)$$

The authors go into more detail of the terms. The TRC term accounts for transaction costs from bid ask spreads. The funding cost is reflected in the exponential term projecting cash flows to time $T$. The authors use weekly one-year time charter rates by Clarkson for a 310,000 dwt VLCC tanker.

Assuming the agents fund the storage trade at LIBOR, they use $r$ as the discount factor, as a proxy. The authors note commodity traders use secured borrowing (via letters of credit for example) which would be at a discount to unsecured rates due to collateral. They plot summary statistics of their dataset:
<center>
<img src="{{ site.imageurl }}/FloatingStorage/stats.png" style="width:100%;"/>
<!-- <figcaption>Required reading.</figcaption> -->
</center>


The authors construct a trade of 1Y horizon from buying the 1M brent contract and selling the 13M Brent contract. They then calculate profits, from 2006-2018. The 13M-1M Brent future spread measures the contango:

<center>
<img src="{{ site.imageurl }}/FloatingStorage/slope.png" style="width:50%;"/>
<!-- <figcaption>Required reading.</figcaption> -->
</center>

This is also easily seen from a forward curve plot when the contango shape is visible in mid 2008:

<center>
<img src="{{ site.imageurl }}/FloatingStorage/contango.png" style="width:55%;"/>
<figcaption>Plot from Ellefsen's Commodity Market Modeling & Physical Trading Strategies paper (2011)</figcaption>
</center>

Assuming a 1Y holding period, the authors plot the weekly PnL from storing 2 million barrels of Brent across time. From the chart, floating storage is profitable only in the end of 2008 and some time in 2015.
<center>
<img src="{{ site.imageurl }}/FloatingStorage/pnl.png" style="width:75%;"/>
<!-- <figcaption>Plot from Ellefsen's Commodity Market Modeling & Physical Trading Strategies</figcaption> -->
</center>

Interestingly, they vary the storage period, with 2M, 6M, 1Y, 18M and 2Y storages, via interpolating their time-charter rate data. This leads to a rather hard to read plot:

<center>
<img src="{{ site.imageurl }}/FloatingStorage/profits_over.png" style="width:65%;"/>
<!-- <figcaption>Plot from Ellefsen's Commodity Market Modeling & Physical Trading Strategies</figcaption> -->
</center>

While difficult to see on the above plot, the authors note that profits with shorter holding periods were higher than longer ones during the financial crisis, but the reverse effect occured in the oil glut. They attribute this effect to the nature of the shock itself affecting time-charter term structure differently.

<center>
<img src="{{ site.imageurl }}/FloatingStorage/gfc.jpg" style="width:45%;"/>
<figcaption>Physical WTI during the GFC.</figcaption>
</center>

In the GFC, oil fell from a high of 133.88 in June 2008 to a low of 39.09 in February 2009. This was a demand shock, as a result of a recession, lower consumer, industrial, air travel and shipping activity activity. The supply of crude is inelastic in the short term. They argue there was low demand across time-charter term structure (flat and low). Since supply didn't decrease as much, there was less need for additional capacity in the form of floating storage. Coupled with the rapid drop in price, it would be better to store for a shorter term.

<center>
<img src="{{ site.imageurl }}/FloatingStorage/og.jpg" style="width:60%;"/>
<figcaption>Brent during the oil glut.</figcaption>
</center>

From reading, the oil glut was caused by the North American shale boom causing oversupply, China's economy slowing down, and the Saudi/OPEC bid to kill off shale by increasing output (production costs of shale very high, requiring a high minimum price to be profitable). This was a supply shock. The authors argue the term structure for TCs was downward sloping as the increased supply led to an increase in demand for floating storage. Thus, only long term holding periods exploiting the far end of the TC curve would be profitable.

## The Floating Storage Decision

The authors then argue that a more accurate assessment involves looking at _excess profits_ over a FFA hedged time-charter strategy - using the time-charter tanker to earn revenue from the voyage charter market instead of just holding crude.

This idea of _optionality_ is explored in Ellefsen's [_Commodity Market Modeling & Physical Trading Strategies_](https://dspace.mit.edu/bitstream/handle/1721.1/61602/704383331-MIT.pdf) 2011 paper, in which the author formulates a model to decide on optimal routing for _cross Atlantic crude oil arbitrage with possibility of floating storage_. 


<center>
<img src="{{ site.imageurl }}/FloatingStorage/ellefsen.png" style="width:80%;"/>
<!-- <figcaption>Brent during the oil glut.</figcaption> -->
</center>

He states this problem of _optimal stopping_ is akin to calculating the value of an _American option_, which gives the owner the right to exercise any time before it's expiry date. This was the original paper I wished to replicate, but was unable to do so due the heavy focus on stochastic calculus and PDEs.

<center>
<img src="{{ site.imageurl }}/FloatingStorage/scatter.png" style="width:80%;"/>
<!-- <figcaption>Brent during the oil glut.</figcaption> -->
</center>

Returning back to the main paper, rather than analytically model the choice with options, they use AIS data from MarineTraffic from 2014-2016. They then manually categorize tankers on time-charter into _storage only_, _storage & voyage_ and _transport_. A tanker is assigned _storage & voyage_ if it is laden & stationary. They measure this by labelling vessels with draught above 15m and speed below 6 knots as stationary.

We can see it does look like positive excess profits over the alternative voyage-charter strategy still happens. 


## Conclusion

To conclude, this post analyzes Regli's _Crude oil contango arbitrage and the floating storage decision_ 2019 paper. While not actually drawing any novel conclusion, the paper has exposed me to a framework of modelling floating storage arbitrage decisions in oil. 

It has also made me more familiar with forward curve term structure and elementary freight concepts. I especially liked the part on using satellite data to classify ship behavior.


