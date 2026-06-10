---
layout: post
title: Football Analytics Ideas
category: FA
---

## Overview

Note: as of May 26 I am building out my data pipelines/datasets/scrapers first which are almost done (though it will expand on a rolling basis as I have more ideas). I am then going to do signal research to test my ideas which are below. My aim is to start by 26/27 (aka September), fundamental 2x1 across big 5 leagues. So, the research and backtesting will be done across the World Cup and pre-season.

For football analytics - I believe there are 3 main problems:


* Overlapping effects - When we see some outcome or effect in football, we know there are a myriad of contributing factors. The question is how to orthogonalize out effects we know to isolate the idiosyncratic return. For example, consider the problem of assessing managers.
* Counterfactuals (Neyman-Rubin) - When we look at stats, they are context dependent (see first point). The team affects the player's stats and the players individual contributions affects the team.
* The Eye Test - How do we quantify what we see with our eyes into statistics? And vice versa? 


## Results Betting

The core would be a **predictive model** that takes in features and outputs signals/predictions, feeding into another **trading layer (risk management/bet sizing)** + a dicsretionary layer of adjustments (e.g from watching games + reading fan forums to understand the situation) to output trades, and we **rebalance/bet weekly**.

The key idea is: how much signal does past information of football matches have in the outcome of the next match?

The core problem is: How do we translate football wisdom or tropes into statistical signals? Tropes that we see play out in matches, before games, across seasons etc. From being a dedicated fan of a team.

The bottom line is, I believe we can see with our eyes and common sense, we know there are certain things that affect the outcomes of matches. My question is - can they be quantified?

### Problems


* **Outlier/Anti-Narrative Problem**
    * A team projected to perform well at the start of a season chokes. Tottenham this season, Liverpool this season. Vice versa: the Bournemouth bull run. In the past, the Leicester bull run. These are outliers.
    * How do you account for outliers in a model? My guess is, if you can't predict them, then don't fight against them. Let the model pick it up and ride the trend.
    * A good test for any model is how well it picks up on the anti-narratives or outliers playing out in real time. For example, if you see the fan channels in 25/26 pre-season, everyone and their mother was glazing Liverpool. Wirtz and Isak are going to destroy the league! Literally nobody could have predicted this. Just like 100 days into the war and oil at 90s... who would have thought?
    * So a model needs to be able to identify these anti-narratives happening in real time and pick up on the trend. 
* **Cold Start Problem**
    * Key Idea - Any points/league weighted feature suffers from the lack of data at the start of a season. Use a weighting scheme from past season.
    * Problems - Plus take into account squad value? Difficult due to lookahead bias of squad/wages
* **Team Strength Problem**
    * Any summarized match/game metric is inflated/deflated by the delta of team strength.
    * For example, a strong team is naturally expected to generate high XG against a weaker team.
* **Game State Weighing Problem**
    * Any summarized match/game metric like XG/XA, PPDA, field tilt, xT are aggregated.
    * These variables fluctuate by game state. Hence, we need time series metrics within game to weigh these metrics.
    * For example, a team going 2-0 up and dominating on XG will sit back in the 70th while the opposing team attacks more. 
    * Idea - Sofascores attacking momentum time series chart.
    * Idea - Possession/ field tilt time series
    * Idea - Score timings as a game state proxy. If we can't get time series possession or field tilt.

### Features

* **Core Strength Feature**
    * Key Idea - The fundamental feature at the start of each season, a team has an innate ranking/strength level from it's players and the manager. This feature should remain relatively stable across the season.
    * Key Idea - core_strength(team) = baseline (existing squad strength last season) + differential(strength of incomings less strength of outgoings).
    * Baseline - The baseline feature, in both season outcomes and baseline odds. Then, you add a differential, and predictive power should improve. 
    * Differential - Strength of incomings (aka the ex-ante transfer problem) and strength of outgoings (this is ex-post).
    * Example - Liverpool's 25/26 window summed it up. Losing Darwin, Diaz, Trent, Jota (RIP), replacing with Frimpong, Kerkez, Wirtz, Ekitike, Isak. Combined with Salah underperfomance, the replacements in reality just failed to fill the gaps of the leavers. They had a high baseline, but such a huge negative differential that it hurt their core strength badly.
* **Core momentum feature**
    * XG - Window of league/point-weighted gamestate-weighted XG differentials - prior XG differentials should encode performance, XG mean reverts to true performance (another hypothesis that needs testing). 
    * XG Over/underperformance of goal diff - Window of league/point-weighted gamestate-weighted goal differentials - Some teams structurally over/underperform their xG, based on style factors (need to research this) or just player power (e.g outrageous goals like PSG)
    * Feature - Window function... that depends. For example, using current league position difference to predict essentially is an expanding sum window over a discretized score outcome.
    * Feature - League/point weighting - Scale the xg difference by points differential. To account for team strength differences.
    * Feature - Game state weighting - For goals, scale them by goal timing: 2-0 with 90' 95' is not the same as 2-0 30' 40'. For XG, create a discretized XG curve and apply a time decay.
* **Lineup/player selection feature**
    * Key Idea - How alpha in picking the "best" starting XI for that opponent contributes to XG and XGA. 
    * Key Idea - The idea that sometimes the managers don't always put out the best starting XI. 
    * Feature - Use historical player ratings to measure effectiveness of the XI - do these capture information? Ratings are within-team relative performance metrics, hence need to scale within team. Also stats vs eye test example (e.g Gyok vs Atletico rated really low by Whoscored but high by BBC). For each league, we have sports journals covering for human ratings (BBC, L'Equipe, Marca, Gazetta, Kicker). 
    * Feature - Win rates with/without. A simple proxy calculation. But of course, need to account for team strength.
    * Connection to Congestion feature - Teams rotating or playing weaker plays to rest for upcoming big games. E.g cup finals.
    * Connection to Availability feature - Injured or unavailable players do not start.
    * Example - Arteta consistently choosing Havertz, Odegaard and Zubimendi and having poor performances despite wins Towards EOS, due to injuries, puts in MLS, Eze, Trossard, the team bangs vs Fulham.
    * Example - For example, MLS started in midfield vs Fulham on 2nd May, replacing Zubi. The algo would rate him low due to poor performances at LB. But at CM, different story (MOTM). So need to scrape and compare historical positions with ratings.
* **Availability feature (injuries + reds + afcon)**
    * Feature - An injury score where player injuries imply the squad is weakened for the next match. This would be tricky because of player importance and substitutability. 
    * Key Idea - We know injuries to a key player can derail an entire season. So this is really important. We've seen Arsenal without Saliba collapsing in the tail end of 22/23, and City without Rodri collapsing mid 25/26 before recovering.
    * Problem - Problem is, need to factor how good the replacement is: aka Nico Gonzalez in for Rodri = no problem. Madueke in for Saka = disaster.
    * Key Idea - The converse can happen! Sometimes we know as fans managers have favorites who get platformed despite performing poorly.
    * Example - Injuries can be a blessing in disguise as the replacements are better than the injured starter! E.g Eze vs Odegaard, we know Eze is superior but Odegaard starts over him.
    * Feature - Player ratings could be used to quantify quality. If the injured starters have worse ratings than the replacements.. maybe injury becomes a boost.
    * Problem - Maybe starting XI is a better proxy? 
* **Manager Feature**
    * Managerial alpha - We know some managers are good. For example, Carrick in 25/26 took United to top 4. How do we account for managerial alpha if we have a manager with no 
    * H2H - Some kind of H2H manager score based on past fixtures over a long period with the same team. Hypothesis being certain managers just tactically have each others number.
    * New manager bounce - Another trope in football. Can this add a boost to certain matches with a new manager bounce? But this is confounded to manager skill track record? 
    * Idea - We can quantify managerial alpha in a possible way. I write about it below. We just need lots of leagues points data, wages and transfer budget in a very wide cross-section.
* **Fatigue/Congestion/Rotation feature (minutes dispersion)**
    * Key Idea - Idea being playing too many games in a tight schedule does impact performance vs well-rested. 
    * Feature - Fixture count, distance ran, degree of rotation/substitutions/minutes played dispersion, all impact fatigue. Especially deep in cup runs.
* **Home and away feature**
    * Feature - A home team score and a away team score.
    * Key Idea - Idea being certain teams have more home/away advantage than others. Andrew Mack's SSMIE book talks about a ZSD model for this. Window of points/league weighted home points vs away points difference
    * Example - Interaction effect with referee, we know sometimes referees get swayed by the home crowd and skew decisions.
* **Referee feature**
    * Some kind of league/point-weighted referee score based on past fixtures over a long period of time with the same time.
    * Idea being that sometimes referees have biases against certain teams.
* **Motivation feature**
    * Key Idea - Some kind of motivation score differential. Idea being that nearing end of season, say last 5 games (GW34), we have teams with increased and decreased motivation.
    * Example - Relegation teams fighting end of the season.
    * Example - Mid table teams with no ability to leapfrog in the table or cannot qualify for the next tier competition (CL, UEL, Conference)
    * Example - Title winners having won already end of season (doesn't happen often in EPL though unlike BL or L1) with no motivation.
    * Example - Sometimes, teams with nothing to play for rest players for a big game. 
    * Feature - Some kind of score that increases or decreases depending on the position in table and number of games left
* **Informed fan feature**
    * Pre match sentiment - Some sentiment indicator of reddit/forum results - only if the informed fans have predictive power.
    * Starting XI reaction sentiment - For example, a bad lineup comes out. Fans on X or the pre-match thread or lineup thread complaining. How do I know? Because I would go immediately to the lineup and start complaining... or at least I would go there to see if other people are complaining.
    * Key Idea - Official club pages will always release XI on X and Instagram. Does the data there have any predictive power as a signal? Perhaps some kind of sentiment score? On Instagram and X.
    * Key Idea - Sentiment score differential against closing odds residual. Or some kind of discount adjustment to the final prediction odds.
    * Key Idea - This is effectively the fan's / wisdom of the crowd view on the starting XI alpha feature. For example, if fans think it is a bad lineup choice, they will be complaining the release thread. 
* **Style feature**
    * The idea that certain teams styles e.g high pressing, possession, counterattack, can neutralize or be (dis) advantaged versus another style. We will use match aggregate statistics for this. But it is subjected to the game state problem: a match statistic is aggregated across the game state time series. 
    * For example, apparently I read on X Hull City who got promoted for 26/27 to EPL really outperformed their XG, and this was a result of their counterattacking style. Possible interaction with XG feature?
 * **Bookie odds feature**
    * Key Idea - The Benter boost as per Mack. Benter in his paper blended his odds/probs with bookie odds/probs in a logit model. Acts as a form of regularization/shrinkage.
    * Other Ideas - Odds dispersion, wrongness, opening vs closing delta. Need to do EDA on odds. Odds time series from bettingiscool (49euro/month).



### The Discretionary Angle  - Open Problems

* **The Player/Transfer Problem**
    * Key Idea - How much strength do incoming players contribute to core squad strength?
    * Example - Kvara signed in Jan 24/25 for PSG for 70m euros. He then made a HUGE impact in PSG for 24/25 and 25/26 CL runs, responsible for their CL dominance.
    * Example - Wirtz signed in July for 25/26 for Liverpool for 120m euros. He then played a HUGE role in flopping (Flopian Wirtz as they call him) and is a reason why Liverpool sucked in 25/26. He couldn't cut it. 120m budget would score high in our feature.
    * Idea: Transfer Coefficient - Some have suggested a "transfer coefficient". Basically quantifying how well players for each position have done in cross-leagues. Then apply that coefficient into a transfer score feature across the season. Market value returns? What about age confounding. Problem: De Bruyne vs Wirtz. Perfect counterexamples.
    * Idea - Discretionary Blend - Scout the transfers and assign a score to each squad. Then apply that to a transfer score across summer/winter. Make intential lookahead bias by forming transfer score feature for teams by looking at ex-post EOS grades (e.g market value). Test if it improves performance. Then if live season, scout and make discretionary scores from YouTube.
    Idea - On the Fly Weighting - Move the transfer problem into the player selection feature. Heavily weigh the new signings ratings more in the strength of squad
* **The Manager Problem**
    * Key Idea - We know certain managers have skill or alpha. They build legacies, they outperform. How can we quantify this, especially if they have limited track records?
    * Example - Carrick signing January to replace a flopping Amorim. They then finish top 4 after Amorim choking.
    * Okay so we do have a possible solution, not perfect, but logical, to kind of assess manager skill over his many tenures. See "managerial assessment" below.

## Research Directions

* Correlative research
    * Standard signal/model research. Based on a hypothesis, quantify the hypothesis, test the idea. Is there predictive power?
* Anti-Correlative Research
    * The key idea here is to find situations/matches where the feature, model (be it yours or bookie) failed or diverged dramatically. 
    * Then do investigative research. What happened there? Could the feature/model have priced it in? If so, what did we miss? If not, how can we neutralize this risk? 

## Modelling Directions

* Rolling window
    * In this approach, the season-fixed features like the manager skill, core strength would remain constant over the season goes but in theory the model should weigh them less naturally as the season goes. A rolling window does this
* Train-Val-Test
    * Not really a big fan of this ...
  

## Managerial Assessment

Quantifying manager performance. We know managers have alpha and beta. Alpha being their idio return and beta being their factor returns (current squad, budget, support). Can we isolate the alpha or idio returns of each manager, netting off the effects of their current squad wages and net spend over their tenure?

The idea would be to see if a manager, over his journey across different clubs, consistently outperforms his budget and wages.

* Regress wages and net spend on league position, across all leagues in cross section, across multiple seasons.
* Again, you can only compare within-league and across time, so you need some relative metric. For example, the net spend of a relegation club in EPL will dwarf the league leaders in, say, the Hong Kong league.
* Each residual in the datapoint is associated with a manager and that season.
* If a manager consistently has a high ranked residual, across tenures, then you have to say maybe...that's the skill.
* Take the managerial alpha residuals and regress against managerial wages.
* Now you have a ranking of which managers have the highest alpha, and also which managers are rich/cheap relative to their alpha.

Data

* A very wide cross-section of wages, transfer spend and league tables across many seasons across many leagues.
* Manager history for a wide cross section
* Managers often jump from lower leagues to higher leagues in their career. For example, Iraola went from AEK Larnaca (Cyprus) to Mirandes (Segunda) to Rayo  Vallecano (Segunda) to Bournemouth and now to Liverpool.

Problem

* Past performance is of course not indicative of future results. A manager can perform well his whole career, move to a new club and flop. That is again a problem in football betting in general...past results != future performance.

## Player Assessment with Stats

How do we optimally make use of statistics to scout players? Say, examine successful players per position - take a look at their stats, extrapolate to scout new players. Like factors driving returns.

* Find a measure of player success per position. E.g market value/wages normalized by age or players, or highest mkt val increase. 
* For the most successful and least successful players: find statistics that impact that the most.
* Can be used as scouting tool for transfers by just looking at statistics

More detailed plan:

* Determine a target variable that is a proxy of player success: For each starting age/year of birth, for each position, for each year, sort players with highest increase in market values forward over next $t$ years. For example.
* Have some treatment for the statistics per player on a team/league level to take into account the influence of team/league on statistics. Aka orthogonalize out against the team/league factors. E.g Wirtz playing in B04, an attacking team, in a farmers league (BuLi).
* Regress residualized statistics across years/seasons against target.
* Take the statistics with most impact / highest coefficients

Problem:

* Finding the right dependent variable is difficult. Many ways to construct it. 
* Statistics generated by players in matches are dependent on their team and league. For example, the argument that Portugese league strikers score tonnes of goals but flop in the higher leagues when they transfer.

## Player Assessment with Physical Attributes

Are there optimal physical metrics per position (arm movement, touch density, leg to torso, torso uprightness) that can determine a player's success in scouting? This relates to the eye test. For example, we can see Messi clearly has some interesting different physical attributes when he plays that contributes to his success. But how to quantify these in numbers is a problem.




## Injuries

Can we predict which teams get the most injuries? 

* Run regression with distance ran and total substitute minutes on injury time per season.
* Should be able to prove that better substitute management results in less injuries.

