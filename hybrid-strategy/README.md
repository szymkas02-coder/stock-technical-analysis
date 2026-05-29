# Hybrid Technical Strategy with Daily Stop-Loss — WSE (GPW) 2000–2025

## Overview

This project backtests a hybrid technical indicator strategy on 354 Warsaw Stock Exchange stocks over 25 years (2000–2025). Each day, every stock receives a composite score from 0 to 1 computed from seven classical technical indicators: RSI(14) and MACD (40% weight combined), ROC(10) — rate of price change (20%), Stochastic %K and CCI — overbought/oversold oscillators (20%), and EMA20 vs. EMA50 trend confirmation (20%). The top 10 scored stocks are held simultaneously. A position is closed when the stock falls below a fixed stop-loss percentage from the purchase price; its slot is immediately filled by the next-best candidate.

## Methodology

Three stop-loss thresholds are compared: −10%, −20%, and −30%. Lookahead bias is avoided: the score is computed on day *t*'s close and trades execute at day *t+1*'s price (the stop-loss is the one exception — it fires on the current session's price, since that is the price crossing the threshold). With this corrected timing, the −10% stop grows 10,000 PLN to 487,536 PLN (+4,775%, win rate 39.6%), the −20% stop to 241,320 PLN (+2,313%, win rate 52.1%), and the −30% stop to 169,984 PLN (+1,600%, win rate 19.0%).

These returns are gross of transaction costs and tax (not modeled in this variant) and are inflated by survivorship bias (the universe is GPW stocks listed today). Reported max drawdown is extreme (~−99%) because positions in stocks that stop trading are carried flat at cost rather than forward-filled — treat the drawdown figure as a data-quality artifact, not a clean risk metric. A follow-up with monthly rebalancing and Belka tax is in `technical-monthly/`.

## Data

- 354 GPW stocks, daily prices 2000–2025
- Starting capital: 10,000 PLN

## Files

- `strategia_hybrydowa_gpw.ipynb` — full daily-cadence backtest notebook
- `artykul_hybrydowa.md` — written article explaining the scoring system and stop-loss comparison (kept local, not tracked by git)
