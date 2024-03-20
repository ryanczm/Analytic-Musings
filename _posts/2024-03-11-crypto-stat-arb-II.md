---
image: "/./assets/images/CryptoStatArb/images_backtest/preview_ts.png"
layout: post
title: "Crypto Stat Arb Series II: Backtesting with a Trade Buffer"
category: quant
excerpt: "Part II of the crypto stat arb series. I code up a backtest in Python based off the Rsims backtesting framework in R (duh) to implement a trading buffer to manage turnover. Then, we backtest the fixed weight combination of carry/momentum/breakout strategy from Part I versus a dynamically weighted version using regressions. With the backtest, we can find an optimal trading buffer value that maximizes Sharpe or turns over a desired portion of the book daily. "
---

Part II of the crypto stat arb series. I code up a backtest in Python based off the Rsims backtesting framework in R (duh) to implement a trading buffer to manage turnover. Then, we backtest the fixed weight combination of carry/momentum/breakout strategy from Part I versus a dynamically weighted version using regressions. With the backtest, we can find an optimal trading buffer value that maximizes Sharpe or turns over a desired portion of the book daily. My code can be found [_here_](https://github.com/ryanczm/Crypto-Stat-Arb).


# Data Preparation

Recall from the previous post our weights were stored in `model_df`, which is in long form sorted by ticker and date.

The absolute values of ticker weights in the universe at any day sum to one. Rather than code up a backtest from scratch and fumbling around, I decided to use `rsims` as a framework. However, I leave out the margin aspects - in the original post, there was no margin calls anyway.

First, the weights dataframe must be converted from long to wide format: where the rows are the date index and columns are all tickers that were ever in the universe.

```python
weights = model_df.pivot(index='date', columns='ticker', values='scaled_weight').fillna(0).reset_index()
close = model_df.pivot(index='date', columns='ticker', values='close').fillna(0).reset_index()
funding = model_df.pivot(index='date', columns='ticker', values='funding_rate').fillna(0).reset_index()
```

<center>
<img src="/./assets/images/CryptoStatArb/images_backtest/0_weights.png" style="width:100%;"/>
</center>

# Structure of the Backtest

The main code of the backtest is in this function: `fixed_commission_backtest_with_funding`. It expects the `weights`, `close` and `funding` in the format mentioned above. It assumes we can trade fractional positions, and starts with 10000 initial cash that is not reinvested - we always rebalance with 10000 or less each day.

The `15 bps` costs is quoted from the original psot as a "reasonable approximation of actual Binance costs (spreads + market impact + commission)".

```python
def fixed_commission_backtest_with_funding(prices, target_weights, funding_rates, 
                                    trade_buffer=0.0, initial_cash=10000, commission_pct=0):

    # Get tickers for later
    tickers = prices.columns[1:]

    # Initial state
    num_assets = prices.shape[1] - 1  # -1 for date column
    current_positions = np.zeros(num_assets)
    previous_target_weights = np.zeros(num_assets)
    previous_prices = np.full(num_assets, 0)
    rows_list = []
    cash = initial_cash
    total_eq = np.array(0)

    # Backtest loop
    for i in range(len(prices)):
        # fetch data
        current_date = prices.iloc[i, 0]
        current_prices = prices.iloc[i, 1:]
        current_target_weights = target_weights.iloc[i, 1:]
        current_funding_rates = funding_rates.iloc[i, 1:]

        # Accrue funding on current positions
        funding = current_positions * current_prices.values * current_funding_rates.values
        funding = np.where(funding == np.nan, 0, funding)
        # PnL for the period: price change + funding
        period_pnl = current_positions * (current_prices.values - previous_prices)
        period_pnl = np.where(period_pnl == np.nan, 0, period_pnl) + funding
        # Update cash balance with period_pnl
        cash += np.sum(period_pnl)
        # Update equity
        cap_equity = min(initial_cash, cash) if not reinvest else cash
        # Update positions based on no-trade buffer
        target_positions = positions_from_no_trade_buffer(current_positions, current_prices, 
                                          current_target_weights, cap_equity, trade_buffer)
        # Calculate position deltas, trade values, and commissions
        trades = target_positions - current_positions

        trade_value = trades * current_prices.dropna().values
        commissions = np.abs(trade_value) * commission_pct
        # After each iteration, set positions to current positions
        current_positions = target_positions
        position_value = current_positions * current_prices.dropna().values

        # Update cash with impact from commissions
        cash -= np.sum(commissions)
        total_eq = np.append(total_eq, cash)

        row_dict = {
            "Date": [current_date] * (num_assets),
            "Close": current_prices.dropna().values,
            "Position": current_positions,
            "Value": position_value,
            "Funding": funding,
            "PeriodPnL": period_pnl,
            "Trades": trades,  
            "TradeValue": trade_value,
            "Commission": commissions,
        }
        rows_list.append(row_dict)
        previous_prices = current_prices
    
    # Combine list of dictionaries into a DataFrame
    result_df = pd.DataFrame(rows_list)
    result_df = result_df.set_index(result_df.Date.apply(lambda x:x[0])).drop(['Date'],axis=1)
    total_eq = pd.Series(total_eq[1:],index=result_df.index)
    sharpe = total_eq.pct_change().mean()/total_eq.pct_change().std()*16
    return result_df, total_eq, sharpe
```

We start off by creating initial state: the `cash`, and `current_positions` array amongst others. In the backtest loop, we fetch the current data: the `current_date`, `current_prices`, `current_target_weights` and `current_funding_rates`. 

The `equity` for the day can be split into three components: the

* `funding` - The total value of carry from our positions in our selected universe.
* `period_pnl` - The PnL from price change of our positions from the current close to yesterday's close. 
* `commissions` - A percentage of the dollar amount traded or `trade_value`, which is simulated to include spread costs and broker commissions.

At the end of each iteration, the calculated data is appended to a list, later to be converted into dataframes. Most importantly, we set `target_positions` to `current_positions` and `current_prices` to `previous_prices` for use in the next iteration of the loop.

## The No-Trade Buffer

The `positions_from_no_trade_buffer` function takes in the `current_positions` (really yesterday's positions) and today's `current_target_weights` and `trade_buffer` value, then produces an array of `target_positions`.

```python
def positions_from_no_trade_buffer(current_positions, current_prices, target_weights, cap_equity, trade_buffer):

    num_assets = len(current_positions)
    target_positions = np.zeros(num_assets)
    current_weights = current_positions * current_prices / cap_equity
    
    for j in range(num_assets):
        if np.isnan(target_weights[j]) or target_weights[j] == 0:
            target_positions[j] = 0
        elif current_weights[j] < target_weights[j] - trade_buffer:
            target_positions[j] = (target_weights[j] - trade_buffer) 
            * cap_equity / current_prices[j]
        elif current_weights[j] > target_weights[j] + trade_buffer:
            target_positions[j] = (target_weights[j] + trade_buffer) 
            * cap_equity / current_prices[j]
        else:
            target_positions[j] = current_positions[j]
    return target_positions

```

The trade buffer is a no-trade region of `2b` around target weight `y`. If our current weight `x` is in the interval of the buffer, we do nothing and do not trade. If it is outside, we trade till the edge of the buffer. Kris references Macrocephalopod's explanation: the idea is to only trade on large enough weight deviations to prevent needless small trades and chip away at equity with transaction costs and fees. 

For example, if our buffer is `0.04`, with current weight `0.12` and target `0.15`, we do nothing since it's still in range. If the target weight is `0.20`, we trade by buying `0.04` of our `cap_equity` for that ticker so our weights are now `0.16`. So, from Monday to Friday, if our weights for a ticker went from `0.16` > `0.18` > `0.15` > `0.12` > `0.14`, we would not trade at all. 

## Picking a Trade Buffer Value

How do we pick a trade buffer value? We can backtest across different values, then pick the value that maximizes Sharpe ratio or that achieves a particular average daily turnover of the book.

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/ceph.png" style="width:55%;"/>
</center>

We optimize over values from `0.00` to `0.08`. With `0.00` meaning no trade buffer and `0.08` meaning the ticker weight must change by at least that amount for a trade to occur.

```python
buff_sharpes = np.array([])
for buffer in [0.00, 0.01,0.02, 0.03,0.04,0.05,0.06,0.07]:
    _,_, buff_sharpe = fixed_commission_backtest_with_funding(close, weights,funding,
    trade_buffer=buffer,
    initial_cash=10000, commission_pct=0.0015,reinvest=False)
    buff_sharpes = np.append(buff_sharpes, buff_sharpe)

pd.Series(buff_sharpes,index=[0.01,0.02, 0.03,0.04,0.05,0.06,0.07])
        .plot(marker='o', figsize=(10,4), title='Sharpe across trading buffer values')
```
<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/1_sharpe_buffer.png" style="width:85%;"/>
</center>

Clearly, `0.05` buffer maximizes our Sharpe ratio. We can see that without the trading buffer, transaction costs significantly negate our edge leading to a Sharpe of `1.1`.

## Backtesting the 0.5/0.2/0.3 Strategy

Now we can feed in our data of our 0.5/0.2/0.3 carry/momentum/breakout strategy into the backtest function to produce our results with a buffer of `0.05` and a comission percentage of `0.15%`, with no reinvesting capital of `10000`.
```python
result_df, total_eq, sharpe = fixed_commission_backtest_with_funding(
                                    close, weights,funding, trade_buffer=0.05,
                                    initial_cash=10000, commission_pct=0.0015, reinvest=False)

resulting_dfs = split_column_lists(result_df, weights.columns[1:].tolist())
close_df = resulting_dfs['Close']
position_df = resulting_dfs['Position']
value_df = resulting_dfs['Value']
funding_df = resulting_dfs['Funding']
periodpnl_df = resulting_dfs['PeriodPnL']
trades_df = resulting_dfs['Trades']
tradevalue_df = resulting_dfs['TradeValue']
commission_df = resulting_dfs['Commission']
```

```python
plot_equity(total_eq,sharpe)
```
<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/2_equity_curve.png" style="width:110%;"/>
</center>


<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/6_rolling_sharpe.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/6_rolling_vol.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/6_drawdown.png" style="width:100%;"/>
</center>


A Sharpe of 1.67, similar to the original post. Let's look at turnover. We can see that with a trade buffer of `0.05` the turnover on average hits `~4%` of the trading capital per day, with extremes of turning over `~15%` of the book. Interestingly, the strategy returns have negative skew. We can also notice that the strategy performs better in more recent years with higher rolling Sharpe ratios and shorter drawdown periods.

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/4_turnover.png" style="width:100%;"/>
</center>



<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/3_rets_dist.png" style="width:90%;"/>
</center>

We can also plot the turnover of a randomly chosen week:


<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/5_daily_turnover.png" style="width:90%;"/>
</center>


## Backtesting a Dynamically Weighted Strategy

In this article, [How to Model Features as Expected Returns](https://robotwealth.com/how-to-model-features-as-expected-returns/), Kris shows how to convert the raw signals into returns. He regresses momentum and carry weights on a historical 90 day window of demeaned returns, and breakout on the same window of returns. The model coefficients are used recalculated every 10 days to capture the changing relationship between deciles and returns. 

We can then blend our carry/momentum/breakout into a single weight for a ticker for a day by taking the model coefficients multiplied by the feature deciles plus the intercept. 

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_research/16_carry_dec_year.png" style="width:100%;"/>
</center>

For example, examining carry deciles against cross-sectional returns over the years reveals a more nuanced trend, but the relationship is still visible.

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_research/15_momo_dec_year.png" style="width:100%;"/>
</center>

Momentum looks even noisier with no visible relationship.


<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_research/17_breakout_dec_year.png" style="width:100%;"/>
</center>

Kris regresses momentum and carry on cross-sectional returns to estimate it, but does not use the linear model for the breakout feature. Instead, we will estimate cross-sectional returns with momentum, carry and breakout with rolling 30 day window then blend the coefficients times weights + intercept for the return estimate, and that will be our weights.

I first rank breakout weights cross-sectionally then use `qcut` to produce our quantiles, then shift it so breakout is on a scale of -4.5 to 4.5:

```python
exp_return_df = model_df.copy()
exp_return_df = exp_return_df.groupby('date').apply(lambda x:x.assign(
                        breakout_weight = pd.qcut(x.breakout_weight.rank(method='first')
                        , q=10, labels=False, duplicates='drop') + 1 - 5.5,
                        )).reset_index(drop=True)
```
Then, we can code up the rolling regressions with `rolling_ols`. We first group by date, then use the length of the grouped data and skip length in our for loop. We store the model coefficients at each interval, then `.resample('D').ffill()` to get daily coefficients.

```python

import statsmodels.api as sm
def rolling_ols(data, target_col, feature_cols, window_size=30, skip_size=1):
    
    label = 'xs' if target_col == 'demeaned_rets' else 'ts'
    results = pd.DataFrame(columns=['date'] + 
    [f'coef_{col}' for col in feature_cols] + [f'{label}_intercept'] + [f'{label}_r2'])
    # Group the data by 'date'
    grouped_data = data.groupby('date')
    # Iterate through the unique dates with the rolling window and skip
    for i in range(0, len(grouped_data), skip_size):
        # Select the unique dates for the current window
        selected_dates = list(grouped_data.groups.keys())[i:i+window_size]
        # Extract rows for the selected dates
        window_data = data[data['date'].isin(selected_dates)]
        # Fit the OLS model
        y = window_data[target_col]
        X = sm.add_constant(window_data[feature_cols])
        model = sm.OLS(y, X).fit()
        # Extract and store the results
        results = results.append({'date': window_data.iloc[-1]['date'],
                                    **{f'coef_{col}': model.params[col] for col in feature_cols},
                                    f'{label}_intercept': model.params['const'],
                                    f'{label}_r2': model.rsquared},
                                    ignore_index=True)

    results.set_index('date', inplace=True)
    results.index = pd.to_datetime(results.index)
    return results.groupby(results.index).last()
```
We can then inner join our regression coefficients and intercept to our data.

```python
xs_coeffs = rolling_ols(df, 'demeaned_rets', ['carry_weight','momo_weight','breakout_weight'])
            .resample('D').ffill()
exp_return_df['date'] = pd.to_datetime(exp_return_df['date'])
exp_return_df = pd.merge(exp_return_df,xs_coeffs,on='date',how='inner')
```
```python
xs_coeffs[['coef_carry_weight','coef_momo_weight','coef_breakout_weight']]
.plot(figsize=(10,4),title='Rolling linear model coefficients')
```

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_research/18_ts_lm_coeffs.png" style="width:100%;"/>
</center>

Similar to the statically weighted strategy, we can run it through the backtest to optimize for the buffer. This time, we get higher volatility and lower returns.

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/1_ts_sharpe_buffer.png" style="width:80%;"/>
</center>


<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/2_ts_equity_curve.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/6_ts_rolling_sharpe.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/6_ts_rolling_vol.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/6_ts_drawdown.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/4_ts_turnover.png" style="width:100%;"/>
</center>

<center>
<img src="{{ site.imageurl }}/CryptoStatArb/images_backtest/3_ts_rets_dist.png" style="width:90%;"/>
</center>

Unfortunately, performance of this strategy is worse than the simple, static 0.5/0.2/0.3 weighting, with 2x as much turnover for the same trading buffer value, a Sharpe ratio of `1.12` for the optimal trading buffer value, and with a very bad and long drawdown for the entirety of 2022. However, the skew profile is positive compared to to the previous strategy. But it definitely seems like keeping thing simple is a much better way to go - especially since the rolling regression coefficients introduces more degrees of freedom into the strategy.

## Summary

And thus, we've backtested the carry/breakout/momentum strategy using signals created in [Part I](https://analytic-musings.com/2024/03/10/crypto-stat-arb-I/), We've constructed a Python event-based backtest using RobotJames and Kris `rsims` backtesting library as a guide. We've backtested two variants of the strategy: one with combining signals in a fixed manner, another with combining signals via estimating expected returns. We've evaluated their equity curves, Sharpe ratios, optimal trading buffer values, return characteristics and turnover.

This concludes a small 2 part series of a trading project. I have no original trading ideas, so the best I can do for now is to copy good projects to learn the workflows.