# Pairs Trading Backtest

This project tests a statistical arbitrage pairs trading strategy using Visa (V) and Mastercard (MA).

The goal is to evaluate whether a mean-reversion strategy based on the price spread between two economically related stocks can generate positive and risk-controlled returns.

Unlike a traditional long-only strategy, pairs trading focuses on relative price movements between two assets rather than the overall market direction.

---

## Project Overview

Pairs trading is a relative-value strategy that looks for temporary divergences between two historically related assets.

The basic idea is:

- If the spread between two related stocks becomes unusually wide, it may revert back toward its historical average.
- The strategy takes a long-short position based on this spread divergence.
- Positions are closed when the spread normalizes.

In this project, I first tested Coca-Cola (KO) and PepsiCo (PEP), but the cointegration relationship was not strong enough. I then tested several economically related stock pairs and selected Visa (V) and Mastercard (MA) based on their strong correlation, cointegration result, and economic similarity.

---

## Research Question

Can a mean-reversion pairs trading strategy based on the spread between Visa and Mastercard generate positive returns with lower volatility and smaller drawdowns than a broad market benchmark?

---

## Data

| Item | Description |
|---|---|
| Data Source | yfinance |
| Period | 2015-01-02 to 2024-12-30 |
| Frequency | Daily adjusted close prices |
| Final Pair | Visa (V) and Mastercard (MA) |
| Benchmark | SPY |

---

## Candidate Pair Selection

Several economically related stock pairs were tested using daily return correlation and cointegration analysis.

Candidate pairs included:

- KO - PEP
- XOM - CVX
- JPM - BAC
- V - MA
- HD - LOW
- MSFT - AAPL
- WMT - TGT
- COST - WMT
- UNH - CI
- GS - MS

The initial KO-PEP pair had strong correlation but weak cointegration:

| Pair | Correlation | Cointegration P-Value | Conclusion |
|---|---:|---:|---|
| KO-PEP | 0.7400 | 0.2849 | Not selected |

The selected pair was V-MA:

| Metric | Value |
|---|---:|
| Correlation | 0.8939 |
| Cointegration P-Value | 0.0000 |
| Hedge Ratio | 0.5409 |

Visa and Mastercard were selected because they showed both strong statistical relationship and economic similarity. Both companies operate in the payment network industry, making the pair more meaningful than a purely statistical match.

---

## Strategy Logic

The strategy uses the spread between Visa and Mastercard:

**Spread = V - Hedge Ratio × MA**

Using the estimated hedge ratio:

**Spread = V - 0.5409 × MA**

The spread is converted into a rolling z-score.

Trading rules:

| Condition | Signal | Position |
|---|---|---|
| Z-score > +2.0 | Short spread | Short V, Long MA |
| Z-score < -2.0 | Long spread | Long V, Short MA |
| Z-score returns to 0.0 | Exit | Close position |

---

## Final Strategy Parameters

| Parameter | Value |
|---|---:|
| Pair | V-MA |
| Rolling Window | 90 days |
| Entry Threshold | ±2.0 |
| Exit Threshold | 0.0 |
| Transaction Cost | 0.10% per position change |
| Hedge Ratio | 0.5409 |

The 90-day rolling window and ±2.0 entry threshold were selected after parameter sensitivity testing. This setting produced a balanced result with fewer trades and lower maximum drawdown compared with more active alternatives.

---

## Methodology

The project follows these steps:

1. Download adjusted close price data using yfinance.
2. Normalize and visualize asset prices.
3. Test the initial KO-PEP pair using correlation and cointegration.
4. Test multiple candidate pairs.
5. Select V-MA based on correlation, cointegration, and economic logic.
6. Estimate the hedge ratio using OLS regression.
7. Calculate the spread between V and hedge-adjusted MA.
8. Convert the spread into a rolling z-score.
9. Generate long-spread and short-spread trading signals.
10. Backtest strategy returns using lagged positions.
11. Apply transaction costs.
12. Run parameter sensitivity testing.
13. Compare final results against SPY.

---

## Key Results

The final V-MA pairs trading strategy achieved:

| Metric | Final V-MA Pairs Strategy |
|---|---:|
| Total Return | 86.75% |
| Annualized Return | 6.71% |
| Annualized Volatility | 8.44% |
| Sharpe Ratio | 0.79 |
| Max Drawdown | -10.22% |
| Win Rate | 23.96% |
| Entries | 35 |
| Exits | 35 |

Aligned cumulative comparison:

| Strategy | Final Cumulative Value |
|---|---:|
| Final V-MA Pairs Strategy | 1.8675 |
| SPY Benchmark | 3.3195 |

---

## Interpretation

The final V-MA pairs trading strategy generated positive returns over the backtest period.

The strategy did not outperform SPY in total return. However, it produced a much smoother risk profile, with lower volatility and a substantially smaller maximum drawdown.

This makes the strategy useful as an example of a lower-risk relative-value approach rather than a pure market outperformance strategy.

The project also shows why pair selection matters. KO-PEP had strong daily return correlation, but its cointegration result was weak. V-MA was selected because it had both strong correlation and strong cointegration.

Main takeaway:

**The strategy worked as a lower-risk relative-value strategy, not as a high-return market-beating strategy.**

---

## Parameter Sensitivity Test

The following parameter combinations were tested:

- Rolling windows: 30, 60, 90 days
- Entry thresholds: ±1.5, ±2.0, ±2.5
- Exit threshold: 0.0
- Transaction cost: 0.10%

Best candidate results:

| Window | Entry Threshold | Total Return | Sharpe Ratio | Max Drawdown | Entries |
|---:|---:|---:|---:|---:|---:|
| 90 | 2.0 | 86.75% | 0.79 | -10.22% | 35 |
| 90 | 1.5 | 108.16% | 0.79 | -11.74% | 52 |

The 90-day / ±2.0 setting was selected because it achieved the same Sharpe ratio as the more active ±1.5 threshold while requiring fewer trades and producing a lower maximum drawdown.

---

## Limitations

This project has several important limitations:

- The final strategy focuses on only one pair.
- The hedge ratio is static and estimated using the full historical period.
- A more realistic approach should use rolling hedge ratios to reduce look-ahead bias.
- Transaction costs are simplified.
- Borrow costs, shorting constraints, bid-ask spreads, and slippage are not fully modeled.
- The strategy does not include stop-loss or take-profit rules.
- SPY is not a perfect benchmark because pairs trading is a long-short relative-value strategy, while SPY is a long-only market benchmark.
- Parameter tuning may introduce overfitting if not validated out-of-sample.

---

## Next Steps

Possible improvements include:

- Testing more candidate pairs across multiple sectors
- Building a multi-pair trading portfolio
- Using rolling hedge ratios
- Adding stop-loss and take-profit rules
- Evaluating trade-level performance
- Including borrow costs and more realistic transaction cost assumptions
- Comparing against a market-neutral benchmark
- Running out-of-sample testing

---

## Tools Used

- Python
- pandas
- NumPy
- matplotlib
- yfinance
- statsmodels

---

## Project Files

```text
pairs-trading-backtest/
│
├── README.md
├── pairs-trading-backtest.ipynb
└── requirements.txt
```

---

## Links

- GitHub: https://github.com/ardabaranbaytar/pairs-trading-backtest
- Kaggle: https://www.kaggle.com/code/ardabaranbaytar/pairs-trading-backtest

---

## Disclaimer

This project is for educational and research purposes only. It is not financial advice.
