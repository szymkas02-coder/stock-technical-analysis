# Momentum Growth Strategy — WSE (GPW) 2015–2025

## Overview

This project tests a multi-horizon momentum scoring strategy on the Warsaw Stock Exchange. Every month, each of 354 GPW stocks receives a score from 0 to 6 points based on three return horizons: 1-month momentum (weight: 3 pts), 12-month momentum (weight: 2 pts), and 36-month momentum (weight: 1 pt). Points are allocated linearly within each horizon. The top 10 ranked stocks enter the portfolio. The headline result: a 1,000 PLN starting capital grew to approximately 13,000 PLN over 10 years (2015–2025).

## Methodology

Three portfolio management variants are compared, differing in rebalancing frequency and entry/exit thresholds. S1 rebalances fully every month (entry threshold 99%, exit 90%). S2 minimizes turnover — only rebalances when the score ranking changes (same thresholds). S3 uses a wider corridor (entry 70%, exit 30%) with change-triggered rebalancing. This separation of scoring logic from rebalancing logic allows measuring the independent impact of turnover and tax drag.

## Data

- 354 GPW-listed stocks, daily prices
- Backtest period: 2015–2025
- Data source: open prices file (`open_prices.csv`, not tracked by git)

## Files

- `strategia_wzrostowa_gpw.ipynb` — full backtest notebook
- `polish_stock_growth.md` — written article describing the strategy and results
