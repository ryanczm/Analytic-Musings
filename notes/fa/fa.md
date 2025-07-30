---
layout: post
title: Football Analytics Ideas
category: FA
---

## Overview

The demand for football is pretty high. Look at salaries. But I'm starting to believe there isn't much intellectual horsepower in it. Why? Probably because nerds don't like football. Well, there is the alpha/arbitrage here. I believe there are 3 main problems:


* Overlapping effects - When we see some outcome or effect in football, we know there are a myriad of contributing factors. The question is how to orthogonalize out effects we know to isolate the idiosyncratic return. For example, consider the problem of assessing managers.
* Counterfactuals (Neyman-Rubin) - When we look at stats, they are context dependent (see first point). The team affects the player's stats and the players individual contributions affects the team.
* The Eye Test - How do we quantify what we see with our eyes into statistics? And vice versa? 

And the data needed would be:

* Competition data - League and cups 
* Transfer/Spending/Salary data - Both at for player and managers
* Statistics - Both at the team and player level.

## Datasets

* Stathead/FBRef ($9/month) - Big tabular database.
* Statsbomb (Seems expensive)

## Player Assessment with Stats

How do we optimally make use of statistics to scout players? Some ideas:

* Examine successful players - take a look at their stats, extrapolate to scout new players. Like factors driving returns.

## Player Assessment with Physical Attributes

Are there optimal physical metrics per position (arm movement, touch density, leg to torso, torso uprightness) that can determine a player's success in scouting? This relates to the eye test. For example, we can see Messi clearly has some interesting different physical attributes when he plays that contributes to his success. But how to quantify these in numbers is a problem.


## Managers

Quantifying manager performance. We know managers have alpha and beta. Alpha being their idio return and beta being their factor returns (current squad, budget, support).

* Can we isolate the alpha or idio returns of each manager, netting off the effects of their current squad wages and net spend?
