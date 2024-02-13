---
layout: post
title: "Scraping QuantTwit for Wisdom"
category: quant
excerpt: "QuantTwit contains a lot of alpha, wisdom by experienced quants. I will scrape, manually, label, clean, whatever, then systematically go through them (need to decide how), learn, and write blogposts to internalize."

---



## Plan

* Idea
    * QuantTwit contains a lot of alpha, wisdom by experienced quants.
    * I will scrape, manually, label, clean, whatever, then systematically go through them (need to decide how), learn, and write blogposts to internalize.
    * Twitter scrape all threads, label with keyword extraction/topic modelling, put in Dataframe
    * Problem: Twitter API Basic plan is $100USD/month
* New Plan:
    * Search by media: look at all pictures, pick out quanty looking ones (basically plots)
    * Search by keyword: EWMA/EMA, Regression, Correlation, Covariance, Filter, Signal, Noise/noisy, Sharpe, Alpha, Scatter, Predict, Adverse Selection, Inference, Statistical, Volatility
* Label (TODO)
    * Subcategory/topic
        * General strategy
        * Actual
        * Domain-specific
    * Difficulty: * | ** | ***
    * Alpha level/importance: * | ** | ***
    * Analysis: put in dataframe and do some data cleaning.
* Post Ideation (TODO)
    * Try and connect or ideate if we can build a post off it: wider context?


## Sources (Populate)

### Macrocephalopod

* Twitter
    * [$TRB trading story](https://twitter.com/macrocephalopod/status/1741822159935185308) (Jan '24)
    * [Short volatility](https://twitter.com/macrocephalopod/status/1747672425586655541) (Jan '24)
    * [Ridge regression tricks](https://twitter.com/macrocephalopod/status/1745040541371322668) (Jan '24)
    * [Trend crossover trick](https://twitter.com/macrocephalopod/status/1750111877265322470) (Jan '24)
    * [Strategy naming](https://twitter.com/macrocephalopod/status/1746581174049251453) (Jan '24)
    * [EMA decomposition](https://twitter.com/macrocephalopod/status/1732097293686354262) (Dec '23)
    * [EMA breakdown](https://twitter.com/macrocephalopod/status/1731986693102633301) (Dec '23)
    * [11 days of backtest errors](https://twitter.com/macrocephalopod/status/1598823745681903616) (Dec '22)
    * [Trend following explained](https://twitter.com/macrocephalopod/status/1587591578331250691) (Nov '22)
    * [Discrete vs Continuous Trading](https://twitter.com/macrocephalopod/status/1535386885408833536) (Jun '22)
    * [Non-overlapping periods](https://twitter.com/macrocephalopod/status/1460308963845677057) (Nov '21)
    * [Quant reversion strategy](https://twitter.com/macrocephalopod/status/1370349076525514752) (Mar '21)
    * [Factor models](https://twitter.com/macrocephalopod/status/1356731277337108482) (Feb '21)
    * [BTC with SPX coupling](https://twitter.com/macrocephalopod/status/1656294685780975617) (Feb '21) 



### RobotJames

* Twitter
    * [VIX commentary](https://twitter.com/therobotjames/status/1756812150084157805) (Feb '24)
    * [What is a Signal?](https://twitter.com/therobotjames/status/1678290394310934529) (Jul '23)
    * [Log returns intuition](https://twitter.com/therobotjames/status/1678259655842344960) (Jul '23)
    * [Artifical data sampling effect](https://twitter.com/therobotjames/status/1671015119575461889) (Jun '23)
    * [Treasury note returns analysis](https://twitter.com/therobotjames/status/1663784977279115265) (May '23)
    * [How to make money trading](https://twitter.com/therobotjames/status/1638292311435034624) (Mar '23)
    * [Equity risk premium timing](https://twitter.com/therobotjames/status/1620331269958815745) (Jan '23)
    * [Scatterplot diagnosis: PE and forward returns](https://twitter.com/therobotjames/status/1460313659381407746) (Nov '21)
    * [Analysing predictive signals](https://twitter.com/therobotjames/status/1332131740580683776) (Nov' 20)
    * [How HFT works](https://twitter.com/therobotjames/status/1513851852953354240) (Apr '22)
    * [Option pricing for degenerate gamblers](https://twitter.com/therobotjames/status/1444832778768379904) (Oct '21)
* Robotwealth Blog
    * [Crypto Stat Arb Series](https://robotwealth.com/quantifying-and-combining-crypto-alphas/) (Feb '24)
* RobotJames YouTube  
    * [Data analysis for traders](https://www.youtube.com/watch?v=Nbq5eyVk-0w&t=4288s&ab_channel=RobotWealth) (Dec '22)
    * [Basics of edge extraction](https://www.youtube.com/watch?v=iDxMhUxnXsg&t=5221s&ab_channel=RobotWealth) (Nov '22)


### Corey Hoffstein

* Twitter
    * [Binary vs Continuous Signal: Cocoa Futures](https://twitter.com/choffstein/status/1755982427720126771) (Feb '24)
    * [Paper link: correlation between assets & portfolio risk](https://twitter.com/choffstein/status/1752040834361393430/photo/1) (Feb '24)
    * [Paper link: Quantica trend following breakdonw](https://twitter.com/choffstein/status/1750604861073142219) (Feb '24)
* ThinkNewFound Blog
    * [Portfolio Tilts versus Overlays: It’s Long/Short Portfolios All the Way Down](https://blog.thinknewfound.com/2023/04/portfolio-tilts-versus-overlays-its-long-short-portfolios-all-the-way-down/) (Apr '23)
    * [Liquidity Cascades](https://www.thinknewfound.com/liquidity-cascades) (Sep'20)
    * [Rebalance Timing Luck](https://www.thinknewfound.com/rebalance-timing-luck) (Aug'20)
    * [Defensive Equity with Machine Learning](https://blog.thinknewfound.com/2020/05/defensive-equity-with-machine-learning/) (May '20)
    * [Why Trend Models Diverge](https://blog.thinknewfound.com/2020/03/why-trend-models-diverge/) (Mar '20)
    * [Payoff Diversification](https://blog.thinknewfound.com/2020/02/payoff-diversification/) (Feb '20)
    * [Macro and Momentum Factor Rotation](https://blog.thinknewfound.com/2019/09/macro-and-momentum-factor-rotation/) (Sep '19)
    * [Decomposing the Credit Curve](https://blog.thinknewfound.com/2019/07/decomposing-the-credit-curve/) (Jul '19)
    * [No Pain no Premium](https://blog.thinknewfound.com/2019/02/no-pain-no-premium/) (Feb '19)
    * [Decomposing Trend Equity](https://blog.thinknewfound.com/2018/09/decomposing-trend-equity/) (Sep '18)
    * [Factor Fimbulwinter](https://blog.thinknewfound.com/2018/06/factor-fimbulwinter/) (Jun '18)
    * [A Trend Equity Primer](https://blog.thinknewfound.com/2018/09/a-trend-equity-primer/) (Sep '18)
    * [Timing Equity Returns with Monetary Policy](https://blog.thinknewfound.com/2018/09/timing-equity-returns-using-monetary-policy/) (Sep '18)
    * [Trade Optimization with MILP](https://blog.thinknewfound.com/2018/08/trade-optimization/) (Aug '18)
    * [Diversifying the What, How When of Trend Following](https://blog.thinknewfound.com/2018/04/diversifying-the-what-how-and-when-of-trend-following/) (Apr '18)
    * [Managing Drawdowns with Trend Following](https://blog.thinknewfound.com/2018/03/protect-participate-managing-drawdowns-with-trend-following/) (Mar' 18)
    * [Are Market Implied Probabilities Useful?](https://blog.thinknewfound.com/2017/11/market-implied-probabilities-useful/) (Nov' 17)
    * [Duration Timing with Style Premia](https://blog.thinknewfound.com/2017/06/duration-timing-style-premia/) (Jun '17)
    * [Anatomy of a Bull Market](https://blog.thinknewfound.com/2017/02/anatomy-bull-market/) (Feb '17)
    * [What are Growth and Value?](https://blog.thinknewfound.com/2016/03/what-are-growth-and-value/) (Mar '16)
    * [Growth is not 'Not' Value](https://blog.thinknewfound.com/2016/02/growth-not-not-value/) (Feb '16)
* FWM Podcast



### El Python Quantador

* Twitter
    * [Tweaking a signal](https://twitter.com/ThePythonQuant/status/1752750066652123167) (Feb '24)
    * [Comparing hypothetical strategy with vol](https://twitter.com/ThePythonQuant/status/1744495480493502613) (Jan '24)
    * [How to scale “things” under what a distribution ”might” look like](https://twitter.com/ThePythonQuant/status/1608607521278918662) (Dec '22)

### QuantyMacro

* Blog
    * [All my homies hate LASSO](https://www.quantymacro.com/all-my-homies-hate-lasso/) (Feb '24)
    * [Combining positions or signals](https://www.quantymacro.com/combinepositionsorsignals2/) (Jan '24)

### Robert Martin

* Twitter
    * [Given a fixed expected value, what hit rate to choose for betting?](https://twitter.com/robertmartin88/status/1566097258805854215) (Sep '22)
    * [Option smiles modelling](https://twitter.com/robertmartin88/status/1532433672162398218) (Jun '22) 
    * [Altcoin beta](https://twitter.com/robertmartin88/status/1461078662233608196) (Nov '21)
    * [Combining signals](https://twitter.com/macrocephalopod/status/1459166928816287751) (Nov '21)
    * [Testing signals/strats on random data](https://twitter.com/robertmartin88/status/1388203875597967361) (May '21)
* Blog
    * [Hypothesis testing in quant finance](https://reasonabledeviations.com/2021/06/17/hypothesis-testing-quant/)(Jun '21)


### Picotrades

* Twitter
    * [Jr Quant ML interview question](https://twitter.com/picotrades/status/1688939629867577344) (Aug '23)
    * [Jr Quant ML interview question 2](https://twitter.com/picotrades/status/1673248470646378498) (Jun '23)
    * [Senior ML quant interview question](https://twitter.com/picotrades/status/1670910638145634306) (Jun '23)


### Formula Delta One

### Andrew Mack
