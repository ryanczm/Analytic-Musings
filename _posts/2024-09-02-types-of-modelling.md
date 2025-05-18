---
layout: post
title: Types of Modelling - Statistical vs Discretionary
excerpt: "So this recently occurred to me while trying to apply statistical modelling to a problem (in my opinion) that was clearly not suited to it. Rather, my prior is that the problem could be solved via discretionary modelling, aka having a hypothesis or thesis about how a series of events in the future would play out."
category: quant
---


So this recently occurred to me while trying to apply statistical modelling to a problem (in my opinion) that was clearly not suited to it. Rather, my prior is that the problem could be solved via discretionary modelling, aka having a hypothesis or thesis about how a series of events in the future would play out.

This got me thinking, what are the different types of modelling approaches? What even is a definition of model? 

## Modelling - What is it Anyway?

I guess this is a fairly safe definition. To model is to make some __prediction about the future, given past data__. I suppose there are 3 main types of models: statistical, discretionary, and numerical.  Statistical modelling refers to exactly what it says on the tin: your regressions, ML, econometrics, whatever. Discretionary modelling refers to constructing an argument or thesis about what someone will do in the future. And numerical refers to simulating a system with known rules/laws forward in time and generating a result. 

With that in mind, we can come up with a nice table:

## A Classification of Models


| Name                                       | Data                                                                                                    | Mechanism                                                                                                                            | Models                             | 
|--------------------------------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Statistical Modelling (Backwards)  | Numerical, tabular records of past observations                                                         | Uncovering statistical relationships (correlations) in past data with the assumption that they can be used for pr ediction in future | Regression, ML, etc                |
| Discretionary Modelling (Forwards) | Current information/events, past history (what actors did in the past given current point-in-time info) | Having a view on what several actors will do in the future and the outcome associated with that                                      | Thesis, view on situation evolving |
| Numerical Modelling                        | Measurements                                                                                            | Knowing the physical laws of a system and simulating them forward in time                                                            | No idea                            |


And:

| Name                    | Skill                                                                                                                    | When to Use                                                                                                                                                                                                                                                                                                | Examples                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| Statistical Modelling   | Understanding underlying mathematical theory, implementing it in code, nature of data and thing you are modelling        | Many participants in a system (e.g millions), individual actor contribution is small, cannot individually predict each of their behavior, BUT. on aggregate, behave in a manner where relationships can be extracted by a statistical model. Relationships are more stationary, aka past predicts future . | Predicting returns                       |
| Discretionary Modelling | Understanding the individual entities and how they will behave based on new/current information and their past behavior. | Relatively fewer participants, individual actors have massive contribution. Actors or events cannot be predicted based on past behavior, e.g lots of random shocks. Past data cannot predict future events or behavior                                                                                     | Thesis or view, say in commodities, macro, politics, etc |
| Numerical Modelling     | Being able to simulate these things in a compute intensive fashion                                                       | Behaviors are fixed                                                                                                                                                                                                                                                                                        | Weather forecasting                      |





## Statistical vs Discretionary

A clear example of the former vs latter and their interplay would be in finance; say quant vs discretionary LS equity. The former might rebalance daily. The latter might rebalance quarterly.

The former uses statistical modelling to model human behavior over a shorter term horizon. The latter uses discretionary modelling or a thesis of how markets would behave to make a decision. But fundamentally, both lead to the same type of action (aka buying or selling an asset).

* Actor Behavior - For statistical vs discretionary, in the former, the aggregate behavior/thesis of actors is simple. In the latter, the behavior of individual actors is very complex and can only be predicted via reasoning.
* Time Horizon - For predictions that are frequent over short horizons, statistical modelling works better. For predictions that are infrequent over long horizons, discretionary works better. This is just repackaging the _fundamental law of active management_.
* Past vs Current Info - If actors consider past information to act, statistical modelling works better. If actors only consider current and future information to act, statistical approaches fail (past doesn't predict future). Discretionary works better. 
* Random Shocks - If there are lots of exogenous random shocks that influence outcomes (e.g geopolitical events, natural disasters, unplanned disruptions) that are current and are hard to be predicted via statistical modelling, then discretionary works better.
* Sample Size - Since discretionary modelling predicts over a longer time horizon, if we were to take that data and model it statistically, say monthly, with 10 features, to get 240 datapoints or rows would require 20 years of data! And we know the world now has changed so much. So sample size is problematic. 

A physical commodity trader told me "We don't rely too much on historical data as markets evolve too quickly". To me, that is clear evidence that discretionary modelling is prevalent in most physical commodities. All the more so for commodities that have high exposure to the geopolitical factor - a very human-centric thing. What kind of statistical model could predict where Israel is going to hit Iran in response to the missile strikes?

## Numerical

Numerical modelling is what we see in applied math/physics, when we build out a set of laws then derive some model (by simulation or theory) to see how a situation or measurements evolve across a time horizon.

How might we deduce these laws? From what I see on X, physics-trained quants are especially good at this. First, they observe the situation. They make assumptions (e.g this phenomenon seems to follow this law) without being too concerned about pinpoint accuracy. They do a quick and dirty estimation, simulating some laws/parameters and them developing a toy model. In this case, there's no statistical element, just some 'rules' that are then applied numerically.

The line between statistical/numerical for DL is blurred but due to a lack of interpretability, let's put it here.

## Integrating Discretionary Views in Statistical Modelling 

Is it possible to blend or combine discretionary views in statistical modelling in a sensible fashion? Perhaps. One could, get a discretionary analyst to create a feature representing his view of some event as a numerical scale (e.g 1-10), then incorporate it into a statistical model.

In a regression, for example, this would let us quantify the impact in terms of the beta of the feature, as well as the correlation to other features. But of course, by definition, a discretionary view is something that is difficult to quantify, hence discretionary, so the scale would be inherently imprecise. But that's about the best way I could see it being done. 

# The Spectrum of Modelling

It's important to remember that all these types of modelling can take place across a spectrum of time horizons - but the speed of execution is constrained by whether a human or computer is executing the model.

**Statistical Examples**

* A HFT firm doing market making at nanosecond speed based on fair value.
* An intraday systematic trading firm doing MFT trading.
* A quant equity firm doing daily stat arb portfolio rebalancing.
* A long term trend following CTA following a quarterly trend and sizing up their position.
* A pharmaceutical researcher doing an RCT to investigate the potential of a drug.
* A social scientist doing a longitudinal study over several years.

**Discretionary Examples**

* An intraday/real time power trader making quick decisions at the desk based on the latest grid data.
* A day trader looking at a chart pattern and manually placeing a trade.
* A football manager looking to make substitution/tactical change. 
* A commodity forward curve trader assessing the latest news on his S/D model to call curve changes.
* A physical trader opting to execute an arbitrage that could involve months of storage/transportation.
* A podshop LS equity manager rebalancing his portfolio to take into account his view.
* A shipowner deciding to timecharter out his vessel for the next decade.
* A private equity firm buying out a company's assets with leverage.

**Numerical Examples**

* A weather model on the ECMWF supercomputer crunching out linear algebra/differential equation solvers.
* A deep learning model at a HFT firm doing inference at lightning speed.
* A physical trader adjusting his SD balance and tallying up the numbers.
* An engineer/physicist looking at his numerical simulation of some phenomonon he is studying.
* A power trading shop running an LP solver to generate a fair value day ahead power price forward curve. 