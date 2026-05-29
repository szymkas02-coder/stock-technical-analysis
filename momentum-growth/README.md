# Momentum Growth Strategy — WSE (GPW) 2015–2025

## Overview

This project tests a multi-horizon momentum scoring strategy on the Warsaw Stock Exchange. Every month, each of 354 GPW stocks receives a score from 0 to 6 points based on three return horizons: 1-month momentum (weight: 3 pts), 12-month momentum (weight: 2 pts), and 36-month momentum (weight: 1 pt). Points are allocated linearly within each horizon. The top 10 ranked stocks enter the portfolio. With no lookahead bias (score on month *t*, trade at month *t+1* prices), the best variant grows a 1,000 PLN starting capital to roughly 8,600 PLN over 10 years (2015–2025).

## Methodology

Three portfolio management variants are compared, differing in rebalancing frequency and entry/exit thresholds. S1 rebalances fully every month (entry threshold 99%, exit 90%). S2 minimizes turnover — only rebalances when the score ranking changes (same thresholds). S3 uses a wider corridor (entry 70%, exit 30%) with change-triggered rebalancing. This separation of scoring logic from rebalancing logic allows measuring the independent impact of turnover and tax drag.

Results (Belka 19% tax applied, no lookahead bias): S1 +77.1% (1,771 PLN), S2 +141.6% (2,416 PLN), S3 +758.7% (8,587 PLN). The wide-corridor S3 wins decisively — its low turnover (207 transactions vs. ~1,500 for S1/S2) minimizes tax drag. Note the universe is stocks listed today, so survivorship bias likely flatters all three.

## Data

- 354 GPW-listed stocks, daily prices
- Backtest period: 2015–2025
- Data source: open prices file (`open_prices.csv`, not tracked by git)

## Files

- `strategia_wzrostowa_gpw.ipynb` — full backtest notebook
- `polish_stock_growth.md` — written article describing the strategy and results (kept local, not tracked by git)
