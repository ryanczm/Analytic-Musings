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


## Results Betting

The traditional sports betting avenue. 

Andrew Mack wrote some sports betting books. Also, knowledge from systematic equity trading should be applicable here.

The core would be a **predictive model** that takes in features and outputs signals/predictions, feeding into another **trading layer (risk management/bet sizing)** + a dicsretionary layer of adjustments (e.g from watching games + reading fan forums to understand the situation) to output trades, and we **rebalance/bet weekly**.

The key idea is: how much signal does past information of football matches have in the outcome of the next match?

The core problem is: You can outplay a team on XG but still lose, and vice versa, and this can happen for streaks - making XG a problem.

There is a situation where the UK bookmakers like Bet365 price odds. To beat those guys, we have to beat the quants from Manchester, Liverpool and London working there.

In terms of features:
* **Points/League Difference**
    * The fundamental feature that a higher placed team should beat a lower placed team. An expanding sum window over a discretized score outcome.
* **Core momentum feature**
    * Window of league/point-weighted time-weighted XG differentials - prior XG differentials should encode performance, XG mean reverts to true performance (another hypothesis that needs testing). 
    * Window of league/point-weighted time-weighted goal differentials - The true realized outcomes of matches, in case teams can consistently outperform/underperform XG due to structural reasons.
    * Window function... that depends. For example, using current league position difference to predict essentially is an expanding sum window over a discretized score outcome.
* **Home and away feature**
    * A home team score and a away team score.
    * Idea being certain teams have more home advantage than others.
    * Certain teams have more away advantage than others.
* **Injury feature**
    * An injury score where player injuries imply the squad is weakened for the next match. This would be tricky because of player importance and substitutability. 
    * We know injuries to a key player can derail an entire season. So this is really important.
    * Player importance: number of matches started and average rating. High starter + high rating out = disaster. Which means we need starting lineups and player ratings.
    * Problem is, need to factor how good the replacement is: aka Nico Gonzalez in for Rodri = no problem. Madueke in for Saka = disaster.
* **Manager H2H feature**
    * Some kind of H2H manager score based on past fixtures over a long period with the same team.
    * hypothesis being certain managers just tactically have each others number.
    * This would be isolated to bigger games, and also managers who have a track record of H2H with the same team.
* **Referee feature**
    * Some kind of league/point-weighted referee score based on past fixtures over a long period of time with the same time.
    * Idea being that sometimes referees have biases against certain teams.
* **Informed fan feature**
    * Some sentiment indicator of reddit/forum results - only if the informed fans have predictive power.


In terms of **football tropes** we see often:

* There is this expectation that if you play well, but you lose, you should in theory win more in the future, vice versa. This should be encoded in XG. For example, Liverpool led the 25/26 season for the first 5-10 games scraping last minute winners etc. We all remember than Szobo dummy and Ngumoha's finish to win vs Newcastle. Then the next game, Crystal Palace tonked them with a last minute winner.
* Big chances or big saves define games. If a shot goes in just a few inches, the whole outcome changes and your whole bet is fried.

In terms of the **core momentum feature**:

* League/point-weighted is a proxy the problem of odds: a higher placed team should beat a lower placed team. An XG against a low point team is less than an XG against a high point team. So you need to calibrate your XG by points differential. Problem is this feature only gains predictive power as the league progresses, at the start its basically useless.
* Time-weighted is a proxy of the idea that last minute winners are not very sustainable. So you weigh your XG by when the goals are scored: a 1-0 in the 1st half and set up shop in the 2nd is much less close than a match with same XG from a last minute winner.
* EWMA or some windowing function should solve the decay problem: recent data is higher weighted because things tend to change. Problem: we know Liverpool 25/26 went on a winning streak in the first few games of the league with last minute winners. They kept winning until they didn't. So how could a model capture when they break the streak and lose?

In terms of **research**:

* XG/Momentum 
    * Look at match threads/data from the most XG-failed matches and try and figure out the pattern.
    * Look at data where teams go on streaks while conflicting their XG. Figure out why the streak happens and why it breaks.
    * Watch the matches with boots on the ground
* Home and Away
    * Backtest this too. Question is how to combine.
* Injury 
    * Backtest this too. Question is how to combine.
* Referee 
    * Backtest
* Sentiment/Informed Fan Factor
    * Backtest this too. Question is how to combine.


In terms of **data**:

* Fixture details - Score, home, away, referee, manager, lineups - FBREF
* Fixture XG - Understat
* Fixture events - Goals timings, red/yellow timings - FBREF
* Injury data - Transfermarket - by player ID. Need to scrape.
* Player ratings - WhoScored - by player ID. Need to scrape. Only current season match ratings but season level ratings over past 5Y.

Quick **ChatGPT data sourcing table**:

| **Source**    | **Fixture Results** | **Personnel (Ref + Managers)** | **Events (Goals/Cards Timing)** | **xG (2 numbers)** | **Injury Data**        | **Player Ratings**    | **API Access**         | **Ease** | **Format**    | **Comments**                                      |
| ------------- | ------------------- | ------------------------------ | ------------------------------- | ------------------ | ---------------------- | --------------------- | ---------------------- | -------- | ------------- | ------------------------------------------------- |
| FBref         | ✅ Yes               | ✅ Yes                          | ✅ Yes                           | ✅ Yes (own model)  | ❌ No                   | ❌ No (raw stats only) | ✅ CSV export           | ⭐⭐⭐⭐⭐    | Clean tables  | **Best backbone**. Use for structure, not ratings |
| Understat     | ✅ Yes               | ❌ No                           | ❌ No                            | ✅ Yes (best xG)    | ❌ No                   | ❌ No                  | ❌ No official API      | ⭐⭐⭐      | JSON (scrape) | Use purely for xG                                 |
| Transfermarkt | ✅ Yes               | ✅ Yes                          | ❌ Limited                       | ❌ No               | ✅ Yes (injury periods) | ❌ No                  | ❌ No                   | ⭐⭐⭐      | HTML tables   | **Best injury source**                            |
| WhoScored     | ✅ Yes               | ❌ No                           | ✅ Yes (very detailed)           | ❌ No               | ❌ No                   | ✅ Yes (0–10 ratings)  | ❌ No                   | ⭐⭐       | JSON in page  | **Best ratings source**, but scraping required    |
| SofaScore     | ✅ Yes               | ❌ No                           | ✅ Yes                           | ❌ No               | ❌ Limited              | ✅ Yes (0–10 ratings)  | ⚠️ Unofficial API      | ⭐⭐⭐⭐     | JSON          | **Best API-style ratings source**                 |
| API-Football  | ✅ Yes               | ✅ Yes                          | ✅ Yes                           | ❌ No xG            | ✅ Yes                  | ❌ No true ratings     | ✅ API (paid/free tier) | ⭐⭐⭐⭐     | JSON          | Clean pipeline, but no ratings                    |
| Sportmonks    | ✅ Yes               | ✅ Yes                          | ✅ Yes                           | ❌ No xG            | ✅ Yes                  | ❌ No ratings          | ✅ API (paid)           | ⭐⭐⭐⭐     | JSON          | Enterprise-level, no ratings                      |

**Python API scrapers**:
* https://github.com/probberechts/soccerdata - 1.7k stars
* https://github.com/collinb9/understatAPI - 23 stars
* https://github.com/dkjorling/FbrefAPI - Deprecated? Worth a try




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

## Managerial Assessment

Quantifying manager performance. We know managers have alpha and beta. Alpha being their idio return and beta being their factor returns (current squad, budget, support). Can we isolate the alpha or idio returns of each manager, netting off the effects of their current squad wages and net spend over their tenure?

* Regress wages and net spend on league position, across all leagues in cross section, across multiple seasons.
* Each residual in the datapoint is associated with a manager and that season.
* Aggregate by summing the residuals per manager tenure.
* Take the managerial alpha residuals and regress against managerial wages.
* Now you have a ranking of which managers have the highest alpha, and also which managers are rich/cheap relative to their alpha.

Problems

* The residuals or league position is not linear in net spend/wages: from 2 > 1 requires more spending/wages than from 20 > 19. How do we normalize for this? Feature transform?
* Other factors apart from wages/net spend: for example, sudden breakout seasons by low-wage players, like Gareth Bale at Tottenham or Michu at Swansea. How do we normalize for this? Use player market value instead of wages?
* Other factors apart from wages/net spend: for example, long term injuries (e.g season ending ligament tears). Add in injury time into the regression? But need to weigh by player impact.. 

## Injuries

Can we predict which teams get the most injuries? 

* Run regression with distance ran and total substitute minutes on injury time per season.
* Should be able to prove that better substitute management results in less injuries.
