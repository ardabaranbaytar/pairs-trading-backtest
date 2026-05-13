# Pairs Trading Backtest

This project tests a pairs trading strategy using Visa (V) and Mastercard (MA).

The goal is to evaluate whether a mean-reversion strategy based on the price spread between two related stocks can generate positive and risk-controlled returns.

## Project Overview

Pairs trading is a market-neutral strategy that looks for temporary divergences between two historically related assets.

In this project, I first tested Coca-Cola (KO) and PepsiCo (PEP), but the cointegration relationship was not strong enough. I then tested several economically related stock pairs and selected Visa (V) and Mastercard (MA) based on their strong correlation, cointegration result, and economic similarity.

## Pair Selection

Several candidate pairs were tested using daily return correlation and cointegration analysis.

The selected pair was:

- Visa (V)
- Mastercard (MA)

The V-MA pair showed:

| Metric | Value |
|---|---:|
| Correlation | 0.8939 |
| Cointegration P-Value | 0.0000 |
| Hedge Ratio | 0.5409 |

## Strategy Logic

The strategy uses the spread between Visa and Mastercard:

```text
Spread = V - Hedge Ratio × MA
