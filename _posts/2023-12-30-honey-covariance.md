---
layout: post
title: "Paper Replication: Honey I Shrunk the Sample Covariance Matrix"
category: quant
excerpt: "Based off Ledoit & Wolfs 2003 paper, which explains the sample covariance matrix is bad. Always shrink it. I perform the same runs of optimization on US stock data from 2005-2022 using both sample and Ledoit-Wolf shrinkage covariance matrices with random excess expected returns. Then we can plot ex-post information ratios, error, etc to compare."
---


In this post, I outline my project based off Wolf & Ledoit's -  _Honey: I Shrunk the Sample Covariance Matrix (2003)_ paper, which showed how shrinking covariance matrices increases realized information ratios & decreases tracking error in active portfolio management then replicate the results on US stocks from 2005-2022.

In this paper, Ledoit & Wolf generate monthly portfolios of different sizes on US stocks from 1983 with randomized excess return forecasts, using a shrunk covariance matrix and a sample covariance matrix, then plot the realized information ratio over different runs. 

Here is the [<i class="fa fa-github" aria-hidden="true"></i> Github repo & code](https://github.com/ryanczm/Honey-I-Shrunk-the-Covariance-Matrix).

_Ledoit, O., & Wolf, M. (2004). Honey, I shrunk the sample covariance matrix. The Journal of Portfolio Management, 30(4), 110–119. [https://doi.org/10.3905/jpm.2004.110](https://doi.org/10.3905/jpm.2004.110)_
<center>
<img src="{{ site.imageurl }}/LedoitWolf/linkedin2.png" style="width:90%;"/>
</center>




## Portfolio Optimization

Ledoit & Wolf conduct the study as follows. At the beginning of the month, they form a value-weighted index of $N$ largest stocks (the benchmark). They feed benchmark weights $W_B$, alphas $\hat{\alpha}$, the covariance matrix $\hat{\Sigma}$ of the last $T=60$ monthly returns, a gain $g$, and an upper bound $c$ into a quadratic optimizer.  

This produces an (active) weight vector $\textbf{x}$. Excess returns are computed as $\textbf{x}^T\textbf{y}$ where $\textbf{y}$ is stock returns. Over the months (1983-2002), they compute the (annualized) _ex-post_ information ratio. The alphas $\hat{\alpha}$ are random by choice, so they repeat the experiment 50 times for any $N$. They then plot IR statistics. The optimization problem is:

$$ \begin{align*}
\text{Minimize:} \quad & \textbf{x}^T \Sigma \textbf{x} \\
\text{such that:} \quad & \textbf{x}^T \alpha \geq g \\
& \textbf{x}^T \mathbf{1} = 0 \\
& \textbf{x} \geq -\textbf{w}_B \\
& \textbf{x} \leq c\mathbf{1} - \textbf{w}_B
\end{align*}$$

The main body of the code with `cvxpy` is as such:

```python
def calculate_active_performance(stocks, benchmark, weights, cutoff=False, shrinkage=True, reduce=True):
  """
  performs singular run using one random alpha vector of portfolio optimization
  """
  T, n, breadth, ir = 60, stocks.shape[1], stocks.shape[1] * 12, 1.5
  active_holdings, active_returns, alpha_list = [], [], []
  df = stocks.iloc[T:]

  for idx, (date, rets) in enumerate(df.iterrows()):
      window = stocks.iloc[idx:idx+T,:]
      vol = window.std() * np.sqrt(12)
      
      """alpha"""
      benchmark_rets = benchmark.iloc[idx+T]
      excess = rets - benchmark_rets.values
      alphas = generate_alphas(excess, ir, breadth, vol)
      
      """covariance matrix"""
      if shrinkage:
        lw = LedoitWolf()
        lw.fit(window)
        cov = lw.covariance_
      else:
        cov = np.cov(window.T)

      """quadratic optimizer"""
      g = (1+300/1e4)**(1/12) - 1 # monthly gain from 300bps annualized gain
      c = 0.10 # max total holdings
      a, w_b = np.array(alphas).flatten(), np.array(weights).flatten()

      x = cp.Variable(n)
      constraints = [
                      a.T @ x >= g,  # Portfolio's expected return should be at least g
                      cp.sum(x) == 0,  # Active weights are dollar neutral
                      x + w_b >= 0,  # Total positions are long only
                      x <= c * np.ones(n) - w_b  # Upper bound on the total weight
                    ]   
      objective = cp.Minimize(cp.quad_form(x, cov))
      problem = cp.Problem(objective, constraints)
      problem.solve()
      x_optimal = x.value
      """..."""
```


In this context, alpha is a cross-sectional forecast of expected excess returns. It's formula comes from Grinold & Kahn: 

$$\alpha = Vol \cdot IC \cdot score$$ 

Scores are created from raw forecasts: $scores=e_t + \epsilon$. Hence, $e_t$ is the realized return for the current month. This is z-scored (cross-sectionally across stocks). Random noise $\epsilon$ is added from a standard normal to get raw scores. $Vol$ is rolling historical vol of excess returns. From the Fundamental Law of Active Management we have

$$IR \approx IC \cdot \sqrt{breadth}$$

Breadth is the annualized number of bets, or $12\cdot N$. The _ex-ante_ information ratio is fixed at 1.5, to which we then back out the information coefficient IC: the assumed correlation between alphas and realized returns. The better an alpha historically predicts excess returns, the more weight it has. We scale by vol because it 'amplifies' the IC. We plug these into the formula to calculate our $\alpha$ vector to be fed.
```python
def generate_alphas(excess, ir, breadth, vol):
      # z-score and add noise to get raw scores
      excess = excess.sub(excess.mean()).div(excess.std())
      raw = excess + np.random.standard_normal()
      # calculate information coeff
      ic = ir / np.sqrt(breadth)
      # convert scale raw scores by IC and historical vol to get alphas
      alphas = ic * raw * vol
      return alphas
```


Other constraints include the portfolio being long only ($\textbf{x} \geq -\textbf{w}_B$), the active weights summing to ($\textbf{x}^T \mathbf{1} = 0$). 


Just like the paper, we run the simulations 50 times each for $N=20, 100, 225, 400$, one using sample covariance, the other using the Ledoit-Wolf estimator and plot our IR statistics.

<center>
<img src="{{ site.imageurl }}/LedoitWolf/realized_ir_replicated.png" style="width:80%;"/>
</center>


Our replication was not exact, as I didn't have access to historical market cap data & historical S&P constituents/weights - only historical prices. This meant I couldn't dynamically construct the benchmark indices across time for different $N$.

```python
def choose_top_n(stocks, weights, n):
  """
  Simulate the large N stocks in Ledoit's paper
  Returns chosen stocks and custom index values based off N stocks
  """
  weights = weights.sort_values('weights',ascending=0)
  weights = weights.iloc[:n,:]
  weights = weights.div(weights.sum())
  stocks = stocks.loc[:,stocks.columns[stocks.columns.isin(weights.index)]]
  return stocks, stocks.dot(weights), weights
```

To make do, we took 400 S&P 'survivors': stocks that remained in the S&P throughout 2005 to 2022, the current S&P benchmark weight composition, and normalized them to sum to 1, then computed a 'custom' S&P index to use as our benchmark.

<center>
<img src="{{ site.imageurl }}/LedoitWolf/realized_ir.png" style="width:90%;"/>
<figcaption>The original boxplot in the paper.</figcaption>
</center>

We notice our plot compared to the original is slightly different, IRs decrease as $N$ increases but the opposite happens for ours. I attribute this to our different replication process with an improvised benchmark, due to data constraints.


## Sample vs Shrunk Covariance

Turning to the covariance aspects of the paper: the formula defined by Ledoit & Wolf for shrinkage is

$$\hat{\Sigma}_{\text{Shrink}} = \delta^* F + (1 - \delta^*) S$$

Where $S$ is sample covariance and $F$ is a structured estimator: the sample constant correlation matrix. We take a convex combination between $S$ and $F$ weighted by $\delta$.

From my understanding, when  $P \gg N$, aka large number of stocks with a small rolling window, the sample covariance matrix $\hat{\Sigma}$ is singular. This relates to rank (denoted by $r$): $r(\textbf{X})=r(\textbf{X}^T)=r(\textbf{X}^T\textbf{X})$. Since $\textbf{X}$ is $n \times p$, it's rank is at most $\min(n,p)$ and so is $\textbf{X}^T\textbf{X}$. 

If so, small eigenvalues will reduce the determinant size, and the inverse matrix will get scaled up. Given the minimum variance portfolio weights vector has a closed form analytical solution involving the inverse, this would propagate the errors to the weights.


To empirically verify this, we take the cosine similarity of $\alpha$ with $\textbf{x}$ over the months, for both sample and Ledoit-Wolf covariance matrices.

<center>
<img src="{{ site.imageurl }}/LedoitWolf/cosine_tgt.png" style="width:100%;"/>
</center>

We can see how shrinkage aligns the weights better with the alphas, having higher cosine similarity (same direction) than the SCM across all time periods. 

In addition, using the SCM in optimization leads to more portfolio turnover. If we take the sum of absolute difference of active weights from $t$ to $t-1$ as turnover like $\sum_i abs(\textbf{x}_t - \textbf{x}\_{t-1})$ and plot it over time, we see the same effect.

<center>
<img src="{{ site.imageurl }}/LedoitWolf/turnover_tgt.png" style="width:100%;"/>
</center>

Thus, we can empirically verify that shrinkage has better alignment with alphas and prevents wild deviations in active weights.


# Conclusion

In conclusion, this post replicates Ledoit & Wolf's _Honey I Shrunk the Sample Covariance Matrix_ 2003 paper. We code up the portfolio optimization procedures on data from 2005-2022 and replicate the _ex-post_ information ratio boxplots to understand why shrinkage leads to better IRs. We then verify that the sample weights deviate further from the alphas and have higher turnover than shrunk weights.

While the paper is focused on risk, my next paper project aims to be one covering expected returns and factor investing (eg Fama/Asness). For a first paper in equities, I think this one was a good place to start.