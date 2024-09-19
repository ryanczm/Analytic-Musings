---
layout: post
title: Types of Modelling - Statistical vs Discretionary
category: quant
---


So this recently occurred to me while trying to apply statistical modelling to a problem (in my opinion) that was clearly not suited to it. Rather, my prior is that the problem could be solved via discretionary modelling, aka having a hypothesis or thesis about how a series of events in the future would play out.

This got me thinking, what are the different types of modelling approaches? What even is a definition of model? 

### Modelling - What is it Anyway?

I guess this is a fairly safe definition. To model is to make some __prediction about the future, given past data__. I suppose there are 3 main types of models: statistical, discretionary, and numerical.  Statistical modelling refers to exactly what it says on the tin: your regressions, ML, econometrics, whatever. Discretionary modelling refers to constructing an argument or thesis about what someone will do in the future. And numerical refers to simulating a system with known rules/laws forward in time and generating a result. 

With that in mind, we can come up with a nice table:

### A Classification of Models


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



Some thoughts:

* For statistical vs discretionary, in the former, the aggregate behavior/thesis of actors is simple. In the latter, the behavior of individual actors is very complex and can only be predicted via reasoning.
* For predictions that are frequent over short horizons, statistical modelling works better. For predictions that are infrequent over long horizons, discretionary works better. This is just repackaging the _fundamental law of active management_.
* If actors consider past information to act, statistical modelling works better. If actors only consider current and future information to act, statistical approaches fail (past doesn't predict future). Discretionary works better. 
