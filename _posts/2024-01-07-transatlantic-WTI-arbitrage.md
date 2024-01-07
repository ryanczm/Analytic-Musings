---
layout: post
title: "Paper Replication: Cost of Carry, Causality & Arbitrage in Oil Futures & Tanker Freight Markets (Alizadeh)"
category: commodities

---
In this post, I analyze  Alizadeh's - Cost of Carry, Causality & Arbitrage between Oil Futures & Tanker Freight Markets 2003 paper, which investigates the existence of excess profits from transatlantic arbitrage by delivering Brent or Bonny  against the WTI futures contract at Cushing due to possible regional supply-demand imbalances. 
<!--more-->
<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/trafigura_commodities_demystified.jpg" style="width:15%;"/>
<figcaption>Required reading.</figcaption>
</center>

<!-- Given my background as an Energy Analyst, it was quite surprising that my time there did not equip me with skills to understand commodity markets. -->
Mainly due to time constraints (did this in 2 days) I did not _replicate_ the paper _per se_, e.g getting the data, coding and evaluating the results. We will do that in due time.


The paper is divided into several parts: it first explains the cost-of-carry model for the WTI Futures Contract, then performs a VECM/cointegration/Granger causality analysis on WTI futures, Brent or Bonny spot, and freight rates data, then 'backtests' to calculate the arbitrage profit. 

# The Cost of Carry Model

WTI is a blend of crudes from Texas, New Mexico, Oklahoma and Kansas. The futures contract is traded at NYMEX/CME.


<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/wti_negative.png" style="width:50%;"/>
<figcaption>Negative WTI futures in April 2020. Unrelated picture.</figcaption>
</center>


WTI futures are _physically deliverable_ into Cushing oil terminal. The catch is, the authors argue, that the contract specifications allow for not just domestic crudes, but _foreign crude_ to be delivered, at a quality discount or premium: 

The authors state (at time of publication), North Sea crudes (BFOE) and Bonny Light (Nigerian) settle at a _$30c/bbl_ discount & _$15c/bbl_ premium respectively to WTI (due to differences in gravity & sweetness). The deliverable quality rulebook by CME is [here](https://www.cmegroup.com/content/dam/cmegroup/rulebook/NYMEX/2/200.pdf).


<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/cushing_map.jpg" style="width:50%;"/>
<figcaption>Cushing terminal, Oklahoma</figcaption>
</center>



<!-- **WTI contract price chart or specification chart** -->

The authors state the (discrete compounding + linear) form of the formula (without convenience yield) relating spot to forward prices $S(t,T)$ and $F(t,T)$ as follows:

$$F_{WTI, t;T}=S_{WTI,t} * (1+C_{WTI, t;T}) + ST_{t;T} + TR + INS$$

Where $C$ is financing costs (interest), $ST$ storage costs, $TR$ is transportation costs (to delivery point) and $INS$ is insurance. If the forward curve is in enough contango and LHS > RHS, then traders can perform a [cash-and-carry storage trade]() to profit off the difference.


<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/sullom_voe.png" style="width:50%;"/>
<figcaption>Sullom Voe, Shetland Islands</figcaption>
</center>

However, given possibility of delivering Brent or Bonny instead of WTI, one can buy sell WTI futures while buying spot Brent and shipping it from Sullom Voe (Brent delivery point) in the Shetland Islands to USA for delivery:

$$F_{WTI, t;T}=S_{BR,t} * (1+C_{BR, t;T}) + ST_{t;T} + TR + INS + 0.30$$

Relating spot Brent to WTI futures, and $TR$ is the corresponding freight rate and $PT$ is the pipeline tariff (from port to Cushing) and the 30c/bbl discount. This formula implies Brent/Bonny spot, WTI futures and the relevant freight rates are linked together _in theory_ as any long-term divergences will be exploited via the above arbitrage.

# Statistical Analysis

The authors pull the weekly close of WTI futures, Brent physical, Nigerian Bonny physical, 1M LIBOR rates from Jan 1993 to Aug 2001 (Datastream). For freight data the authors use weekly rates of Aframax Sullom Voe to Bayway (NSEA to USAC) and Suezmax Bonny to Philadephia (WAFR to USAC) in Worldscale, then converted to USD/bbl (Clarksons).

Looking at the [Baltic Exchange Website interactive map](https://www.balticexchange.com/en/data-services/routes.html) for dirty tanker routes (TD), we see this map:

<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/routes.png" style="width:70%;"/>
<figcaption>While the interface is poor, the only two TD routes from West Africa are not direct to US Gulf.</figcaption>
</center>

I notice the exact routes stated by the authors in 2003 no longer exist on the Baltic website. For Brent, the closest routes are `TD7` (Aframax from North Sea to Continent) and `TD25` (Panamax from ARA to US Gulf). For Bonny, `TD20` (Suezmax from West Africa to UK continent). One assumes charterers use composite routes. Looking closely, there is no blue line from West Africa to the US in today's map.

<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/cc.jpg" style="width:40%;"/>
<figcaption>Map from Argus Media (2020). Pipelines to Cushing</figcaption>
</center>

The authors plot F1WTI (contract closest to expiry), Bonny and Brent to notice a close relationship. This was the _only_ chart in the paper.

<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/prices.png" style="width:80%;"/>
<figcaption>WTI futures (near month), physical Brent and Bonny, 1992-2001</figcaption>
</center>

They then perform a myriad of stationarity tests and a normality test on the logged prices/diffs series to verify the $I(1)$ property (differenced series is stationary) needed for cointegration tests. Then, they use the Johanssen cointegration test to derive cointegrating vectors, then plug them into a VECM model:

$$\Delta X_t = \sum_{i=1}^{p-1}\Gamma_i \Delta X_{t-i}+ \Pi X_{t-1}+\epsilon_t$$

The $\Delta X_t$ is a first-differenced vector of WTI futures, Brent (or Bonny), and the corresponding freight rate. To fully understand the paper, we would need to _learn the logic of cointegration tests, VECM & Granger causality tests (I don't claim to!)_ which would take _a lot_ of time. I will mark this as todo in the future.
<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/panel.png" style="width:80%;"/>
<figcaption>Panel of VECM/GC tests for WTI, Brent, Freight.</figcaption>

</center>

While the authors provide several analyses, the most important point states:

> The level of freight rates were not affected by the differential of WTI futures with either Brent or Bonny.
 
In theory, if the WTI futures-imported crude spread increases, Brent is cheaper to deliver, and traders would charter ships to bring imported crude to the USA to arbitrage, causing freight rates to increase. But clearly, this was not the case! A referee then pointed out tanker freight rates not responding to the spread movements could be due to excess capacity. Nonetheless, this still implies arbitrage is possible.

To investigate, the authors backtest the results of the Brent and Bonny trades across various parameters and tabulate the results. They use the spot price of Brent 3,4,5 weeks prior to maturity of the front WTI futures contract: delivery to Sullom Voe takes up to 2 weeks, loading takes 4 days, shipping takes 9, and discharging takes 4.

<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/res.png" style="width:80%;"/>
<figcaption>Brent panel.</figcaption>
</center>

If one squints hard enough, the Bonny trades are significantly more profitable than the Brent trades, on a per-trade basis. They conclude Bonny is ideal for this trade.

<center>
<img src="{{ site.imageurl }}/TransatlanticWTIArbitrage/res2.png" style="width:80%;"/>
<figcaption>Bonny panel.</figcaption>
</center>
To further investigate, we could find data showing the proportions of different grades of crude delivered against WTI at Cushing over time, then plot out the corresponding freight rate prices & futures-spot differential to observe any trends. Having said that, while futures & spot crude data is easily available, the correct freight data is not.

## Conclusion

In conclusion, this post analyzes Alizadeh's _Cost of Carry, Causality & Arbitrage in Oil Futures & Tanker Freight Markets_ 2003 paper. While not being able to replicate the results due to data and time constraints, I will earmark this as a future project to try on out-of-sample data. This paper has exposed me to some concepts in commodity trading, showing how spatial arbitrage is done in theory.