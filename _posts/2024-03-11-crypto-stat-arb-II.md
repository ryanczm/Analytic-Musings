---
layout: post
title: "Crypto Stat Arb Series II: Backtesting with a Trade Buffer"
category: quant
excerpt: "Part II trading project from RobotJames/Kris, originally in R, in Python. I code up a backtest in Python based off RJ/Kris' R backtesting framework Rsims to implement a trading buffer as a heuristic to manage turnover. I then backtest the 0.3/0.2/0.5 carry/momentum/breakout weighted strategy from Part I versus a dynamically weighted version modelling expected returns, finding the trade buffer value for optimal Sharpe."

---

Part II trading project from RobotJames/Kris, originally in R, in Python. I code up a backtest in Python based off RJ/Kris' R backtesting framework [Rsims](https://github.com/Robot-Wealth/rsims) to implement a trading buffer as a heuristic to manage turnover. I then backtest the 0.3/0.2/0.5 weighted strategy from Part I versus a dynamically weighted version of the strategy from modelling expected returns.

We want to simulated our strategy in the previous post via a backtest. RobotJames and Kris cover this in this part of their Crypto stat arb series. They have a backtesting library in R, `rsims`, that they use and demonstrate in their post. We are going to implement a simpler version of `fixed_comission_with_funding.R` in Python. To quote their post:

>This time we’ll consider costs and constrain the universe so that it would be operationally feasible to trade. And we’ll use an accurate backtest to explore the various trade offs.

# Data Preparation

Recall our weights were stored in `model_df`:

$$Model \space DF$$

Where the absolute values of ticker weights in the universe at any day sums to one. Rather than code up a backtest from scratch and fumbling around, I decided to use `rsims` as a framework. However, I leave out the margin aspects - in the original post, there was no margin calls anyway.

First, the weights dataframe must be converted from long to wide format: where the rows are the date index and columns are all tickers that were ever in the universe.

```python
weights = model_df.pivot(index='date', columns='ticker', values='scaled_weight').fillna(0).reset_index()
close = model_df.pivot(index='date', columns='ticker', values='close').fillna(0).reset_index()
funding = model_df.pivot(index='date', columns='ticker', values='funding_rate').fillna(0).reset_index()
```

$$Weights$$

# Structure of the Backtest

The function `fixed_commission_backtest_with_funding` expects the `weights`, `close` and `funding` in the format mentioned above. It assumes we can trade fractional positions, and starts with 10000 initial cash that is not reinvested - we always rebalance with 10000 or less each day.

```python
def fixed_commission_backtest_with_funding(prices, target_weights, funding_rates, trade_buffer=0.0, initial_cash=10000, commission_pct=0, reinvest=True):

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
        # Update cash balance - includes adding back yesterday's margin and deducting today's margin
        cash += np.sum(period_pnl)
        # Update equity
        cap_equity = min(initial_cash, cash) if not reinvest else cash
        # Update positions based on no-trade buffer
        target_positions = positions_from_no_trade_buffer(current_positions, current_prices, current_target_weights, cap_equity, trade_buffer)
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

We start off by creating initial state: the `cash`, and `current_positions` Numpy array. In the backtest loop, we fetch the current data: the `current_date`, `current_prices`, `current_target_weights` and `current_funding_rates`. 

The `equity` for the day can be split into three components: the

* `funding` - The total value of carry from our positions. 
* `period_pnl` - The PnL from price change of our positions from the current close to yesterday's close. 
* `commissions` - A percentage of the dollar amount traded or `trade_value`, which is simulated to include spread costs and broker commissions.

At the end of each iteration, the calculated data is appended to a list, later to be converted into a `DataFrame`. Most importantly, we set `target_positions` to `current_positions` and `current_prices` to `previous_prices` for use in the next iteration of the loop.

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

    return target_positions

```

The trade buffer is a no-trade region of `2b` around target weight `y`. If our current weight `x` is in the interval of the buffer, we do nothing and do not trade. If it is outside, we trade till the edge of the buffer. Kris references Macrocephalopod's explanation: the idea is to only trade on large enough weight deviations to prevent needless small trades and chip away at equity with transaction costs and fees. 

For example, if our buffer is `0.04`, with current weight `0.12` and target `0.15`, we do nothing since it's still in range. If the target weight is `0.20`, we trade by buying `0.04` of our `cap_equity` for that ticker so our weights are now `0.16`. So, from Monday to Friday, if our weights for a ticker went from `0.16` > `0.18` > `0.15` > `0.12` > `0.14`, we would not trade at all. 