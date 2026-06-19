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

The bottom line is, I believe we can see with our eyes and common sense, we know there are certain things that affect the outcomes of matches. We can observe certain **narratives** or **subplots** playing out over the course of a season My question is - can they be quantified?

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
* **XI Release Timing Problem**
    * Do you go in before the XI is realised or right after?
    * You can have two variants of timings/approaches where in the former you take injuries to forecast availability.
    * Or you can wait for the XI to release and have that information then go in.
    * If you price XI information better than the market, then you should go in after. 
    * So there are two timing approaches for any fundamental strategy. Which one makes more money?
* **Counterexample Problem**
    * No matter how much you can find a trend or signal, there are always counterexamples.
    * E.g transfer coefficient. Wirtz vs DeBruyne, Buli CAM to EPL. One flops, the other becomes a legend.
 

### Features

* **Core Strength Feature**
    * Idea - The fundamental feature at the start of each season, a team has an innate ranking/strength level from it's players and the manager. This feature should remain relatively stable across the season.
    * Core Strength Equation - core_strength(team) = baseline (existing squad strength last season) + differential (ex-ante expected contribution/strength of incomings less ex-post contribution/strength of outgoings).
    * Evaluation - In season outcomes and baseline win rates/odds (not residual odds), since this is a "beta" feature, not "alpha".
    * Modelling - All separate features (e.g their own units/dimensions) or single (better for interpretation but harder to do)
    * Baseline - Past squad strength. Past position/points, squad value, strength scores, etc.
    * Differential - Strength of incomings (aka the ex-ante transfer problem) and strength of outgoings (this is ex-post).
    * Spending Power - Within league relative or past-position relative (for this position, say midtable, how much is usually spent?), total spent or net spend
    * Incomings - I talk about possible approaches to the ex-ante transfer problem below. For ex-ante, we have market value, prices, player ratings at past clubs. But...
    * Outgoings - Relative player performance per game, accumulated across a season, scaled to baseline or points as a measure of contribution.
    * Problem - Both baseline and differential (ins and outs) must be comparable within league and the same units! In other words, the relative strength of incomings at Liverpool must be comparable to relative strength of incomings at Bournemouth, for example. Same for relative strength of outgoings at Liverpool and Bournemouth.
    * Example - Liverpool's 25/26 window summed it up. Losing Darwin, Diaz, Trent, Jota (RIP), replacing with Frimpong, Kerkez, Wirtz, Ekitike, Isak. Combined with Salah underperfomance, the replacements in reality just failed to fill the gaps of the leavers. They had a high baseline, but such a huge negative differential (weak ins and strong outs) that it hurt their core strength badly. The question is how we quantify that. Remember - consensus ex-ante 25/26 was the ins were really really strong.
* **Core momentum feature**
    * Idea - Window of league/point-weighted gamestate-weighted XG differentials - prior XG differentials should encode performance, XG mean reverts to true performance (another hypothesis that needs testing). 
    * XG Over/underperformance of Goal Diff - Window of league/point-weighted gamestate-weighted goal differentials - Some teams structurally over/underperform their xG, based on style factors (need to research this) or just player power (e.g outrageous goals like PSG)
    * Window Function - Equal weight (SMA), exponential decay (EMWA). For example, using current league position difference to predict essentially is an expanding sum window over a discretized score outcome.
    * Team Strength Delta - Marginal XG value varies in opponent strength. Scale the XG difference by team strength deltas. To account for team strength differences.
    * Game State Weighting - Marginal XG value varies in time. E.g for goals, scale them by goal timing: 2-0 with 90' 95' is not the same as 2-0 30' 40'. For XG, create a discretized XG curve and apply a time decay.
* **Lineup/player selection feature**
    * Idea - How much alpha in picking the selected starting XI / subs for that opponent contributes relative to the theoretical ideal/maximum starting XI/subs. The idea sometimes managers do not pick the "best" lineup.
    * Idea - These are all (creative) functions of player ratings, assuming they are indicative of performance and past performance carries into future performance. Player ratings are proprietary functions of events/tracking data in the algo sense, and journalist human views (eye tests). We use ratings (for now) because we do not have detailed events data, nor do we have a view on how to process events/tracking data to assess performance better than the ratings providers.
    * Ratings - Use historical player ratings to measure effectiveness of the XI. Statistical/algo ratings vs journal ratings.  
    * Variant - Weighing new transfers more, as existing players have already a baseline strength from last season.
    * Variant - Weighing recent form more vs past historical form (e.g past seasons ratings with large $n$). Important for not-nailed on starters (aka Skelly in midfield banged vs Fulham replacing Zubimendi, but was poorly rated at LB in the past)
    * Variant - Weighing z-scores by position group (attackers, midfielders, defenders, GK). Relative performance to your position group instead of your squad.
    * Variant - Weighing journal vs algo. How much weight do you put in a journalist view vs an algo view? Stat padders might get artificially boosted by algos (see Gyokeres in Arsenal Atletico UCL Semi 1st leg 25/26, rated highly by BBC/Sky and fans but poorly by the algorithm. Not good.)
    * Variant - Theoretical ideal lineup construction algorithm
* **Availability/injury feature**
    * Idea - In theory, injuries to key players can damage outcomes. Afcon is basically an injury.
    * Idea - Should be captured in player selection, but what if the ratings price a key player lower?
    * Injury Separation from Player Selection - For that reason, should have an injury feature that quantifies the importance of injured players. And we would use player ratings over historical seasons (if there any) to get this importance. What if it's a new transfer?
    * Replacement Quality Problem - In theory, a separate injury score and a player selection feature should solve the problem of the injury replacement quality option. (E.g when Ruben Dias got injured, City brought in Max Alleyne and choked out some losses on the tape before bringing Guehi in January. Arsenal's Saliba injury in 22/23 where Rob Holding stepped up caused a title collapse. In other words - the quality of the replacement can mitigate or exacerbate the injury loss.)
* **Manager Feature**
    * Managerial Alpha - We know some managers are good. For example, Carrick in 25/26 took United to top 4. How do we account for managerial alpha if we have a manager with no 
    * H2H - Some kind of H2H manager score based on past fixtures over a long period with the same team. Hypothesis being certain managers just tactically have each others number.
    * New Manager Bounce - Another trope in football. Can this add a boost to certain matches with a new manager bounce? But this is confounded to manager skill track record? Usually happens as a firing manager is underperforming, by definition.
* **Fatigue/Congestion/Rotation feature (minutes dispersion)**
    * Idea - Idea being playing too many games in a tight schedule does impact performance vs well-rested. 
    * Example - Need to find examples where teams bust a gut early in the season, and towards the tail end, they collapse (e.g Arteta & Klopp Arsenal April collapse). Usually accompanied by a congested schedule and injury pileup.
    * Feature - Fixture count, distance ran, pressing intensity, degree of rotation/substitutions/minutes played dispersion, all impact fatigue. Especially deep in cup runs. Kind of kicks in the last months of a season with a drastic collapse. Hence the birth of the April collapse.
* **Motivation feature**
    * Idea - Some kind of motivation score differential. Idea being that nearing end of season, say last 5 games (GW34), we have teams with increased and decreased motivation.
    * Example - Relegation teams fighting end of the season.
    * Example - Mid table teams with no ability to leapfrog in the table or cannot qualify for the next tier competition (CL, UEL, Conference)
    * Example - Title winners having won already end of season (doesn't happen often in EPL though unlike BL or L1) with no motivation.
    * Example - Sometimes, teams with nothing to play for rest players for a big game. 
    * Feature - Some kind of score that increases or decreases depending on the position in table and number of games left
* **Style feature**
    * Idea - The idea that certain teams styles e.g high pressing, possession, counterattack, can neutralize or be (dis) advantaged versus another style. We will use match aggregate statistics for this. But it is subjected to the game state problem: a match statistic is aggregated across the game state time series.
    * Bogey Matchups - The football trope of the bogey team. Commonly found on fan forums (e.g reddit) as "folk wisdom" from watching games. E.g City somehow always roll over to Spurs... Why? A stylistic mismatch (possession vs counterattack specialists). E.g  City vs United 0-2, a weakness to counterattacks (1st goal) and crossing (2nd goal).
    * Example - For example, apparently I read on X Hull City who got promoted for 26/27 to EPL really outperformed their XG, and this was a result of their counterattacking style. Possible interaction with XG feature?
* **Home and away feature**
    * Feature - A home team score and a away team score.
    * Key Idea - Idea being certain teams have more home/away advantage than others. Andrew Mack's SSMIE book talks about a ZSD model for this. Window of points/league weighted home points vs away points difference
    * Example - Interaction effect with referee, we know sometimes referees get swayed by the home crowd and skew decisions.
* **Referee feature**
    * Some kind of league/point-weighted referee score based on past fixtures over a long period of time with the same time.
    * Idea being that sometimes referees have biases against certain teams.
* **Informed fan feature**
    * Pre match sentiment - Some sentiment indicator of reddit/forum results - only if the informed fans have predictive power.
    * Starting XI reaction sentiment - For example, a bad lineup comes out. Fans on X or the pre-match thread or lineup thread complaining. How do I know? Because I would go immediately to the lineup and start complaining... or at least I would go there to see if other people are complaining.
    * Key Idea - Official club pages will always release XI on X and Instagram. Does the data there have any predictive power as a signal? Perhaps some kind of sentiment score? On Instagram and X.
    * Key Idea - Sentiment score differential against closing odds residual. Or some kind of discount adjustment to the final prediction odds.
    * Key Idea - This is effectively the fan's / wisdom of the crowd view on the starting XI alpha feature. For example, if fans think it is a bad lineup choice, they will be complaining the release thread. 
 * **Bookie odds feature**
    * Key Idea - The Benter boost as per Mack. Benter in his paper blended his odds/probs with bookie odds/probs in a logit model. Acts as a form of regularization/shrinkage.
    * Other Ideas - Odds dispersion, wrongness, opening vs closing delta. Need to do EDA on odds. Odds time series from bettingiscool (49euro/month).



### The Discretionary Angle  - Open Problems

* **The Player/Transfer Problem**
    * Key Idea - How much strength do incoming players contribute to core squad strength?
    * Example - Kvara signed in Jan 24/25 for PSG for 70m euros. He then made a HUGE impact in PSG for 24/25 and 25/26 CL runs, responsible for their CL dominance.
    * Example - Wirtz signed in July for 25/26 for Liverpool for 120m euros. He then played a HUGE role in flopping (Flopian Wirtz as they call him) and is a reason why Liverpool sucked in 25/26. He couldn't cut it. 120m budget would score high in our feature.
    * Idea - Past player performance - with player ratings (requires increasing dataset universe of player ratings and scores/outcomes), transfer coefficient, UEFA coefficient. Problem - cross league adaptability is a big problem! Need to account for this somehow.
    * Idea - Transfer Coefficient - Some have suggested a "transfer coefficient" + UEFA coefficients.
    * Idea - Discretionary Blend - Scout the transfers and assign a score to each squad. Then apply that to a transfer score across summer/winter. Make intential lookahead bias by forming transfer score feature for teams by looking at ex-post EOS grades (e.g market value). Test if it improves performance. Then if live season, scout and make discretionary scores from YouTube.
    * Idea - On the Fly Weighting - Move the transfer problem into the player selection feature. Heavily weigh the new signings ratings more in the strength of squad
    * The Wirtz Problem - So if you check [this link](https://www.reddit.com/r/soccer/comments/1l89qfn/romano_florian_wirtz_to_liverpool_here_we_go/), you have fans glazing that mfer. But scroll down, at -7 downvotes, a comment says "He has good league numbers, but most of his production came against teams like Bremen, Freiburg, and Wolfsburg. He was disappointing in the Europa league final as well as the Euros. In last two seasons he hasn't scored or assisted against Bayern in the league as well. Then in UCL didn't play well vs Atleti, Liverpool, Bayern, with his best perfs against Brest and Feyenoord". At -7. So this guy, has successfully ex-ante made the Wirtz call. The footballing equivalent of calling the US/Iran MoU in April during peak oil. This comment says he doesn't show up in big games therefore flop. Now this gets me thinking.... can we quantify this?
* **The Manager Problem**
    * Key Idea - Quantifying manager performance. We know managers have alpha and beta. Alpha being their idio return and beta being their factor returns (current squad, budget, support). Can we isolate the alpha or idio returns of each manager, netting off the effects of their current squad wages and net spend over their tenure?
    * Key Idea - Regress wages and net spend on league position, across all leagues in cross section, across multiple seasons.  If a manager consistently has a high ranked residual, across tenures, then you have to say maybe...that's the skill.
    * Key Idea - However, important to remember when you include managerial alpha in the model, the alpha must be re-calculated to exclude everything after and the current season you are modelling! If not, lookahead bias. 
    * Problem - Does not take into account the intrinsic value of transfers over/under their budget. E.g was Iraola carrying Bournemouth or was Rayan carrying Iraola?

## Research Directions

* **Correlation Directions**
    * Correlative research
        * Standard signal/model research. Based on a hypothesis, quantify the hypothesis, test the idea. Is there predictive power?
    * Anti-Correlative Research
        * The key idea here is to find situations/matches where the feature, model (be it yours or bookie) failed or diverged dramatically. 
        * Then do investigative research. What happened there? Could the feature/model have priced it in? If so, what did we miss? If not, how can we neutralize this risk? 
* **Correlation Types**
    * Season correlations
        * For season-level features, we have the extra option of scattering against season outcomes (e.g points or league ranking)
    * Fixture correlations
        * Where we take the difference in two scores of a feature for home and away and use that is a predictor of win probabilities.

## Modelling Directions

* **Setup** 
    * Rolling window
        * In this approach, the season-fixed features like the manager skill, core strength would remain constant over the season goes but in theory the model should weigh them less naturally as the season goes. A rolling window does this
    * Train-Val-Test
        * Not really a big fan of this ...
* **Features**
    * Aggregation/score rollup vs individual - What if instead of putting features individually, we come out with a score/index from modelling groups of features, then feed it into the final model?
    * Interpretability - Much better and we what see on the pitch.
    * Rollup scores - Manager score, squad strength score (baseline + differential), momentum/form score, player selection/injury score, fatigue/congestion score, style score.
    * Attribution - E.g lose game because of big injuries. Lose game because looking tired in the 2nd half due to season-accumulating fatigue. Lose game because tactically outmatched by a style.



## Datasets Not Spanned

* Complex in-game play-by-play events data, from StatsBomb or Opta. Proprietary, used by scouts, expensive.
* PLayer in-game statistics. Per game, aggregated from the former. Do these have any use? It tells you how a player performed / what he did in a game, beyond just the player ratings. Ratings are a function of this.


## Non-Betting Ideas

### Player Assessment with Stats

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

### Player Assessment with Physical Attributes

Are there optimal physical metrics per position (arm movement, touch density, leg to torso, torso uprightness) that can determine a player's success in scouting? This relates to the eye test. For example, we can see Messi clearly has some interesting different physical attributes when he plays that contributes to his success. But how to quantify these in numbers is a problem.

### Injuries

Can we predict which teams get the most injuries? 

* Run regression with distance ran and total substitute minutes on injury time per season.
* Should be able to prove that better substitute management results in less injuries.

