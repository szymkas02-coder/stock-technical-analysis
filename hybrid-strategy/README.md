# Hybrid Technical Strategy with Daily Stop-Loss — WSE (GPW) 2000–2025

## Overview

This project backtests a hybrid technical indicator strategy on 354 Warsaw Stock Exchange stocks over 25 years (2000–2025). Each day, every stock receives a composite score from 0 to 1 computed from seven classical technical indicators: RSI(14) and MACD (40% weight combined), ROC(10) — rate of price change (20%), Stochastic %K and CCI — overbought/oversold oscillators (20%), and EMA20 vs. EMA50 trend confirmation (20%). The top 10 scored stocks are held simultaneously. A position is closed when the stock falls below a fixed stop-loss percentage from the purchase price; its slot is immediately filled by the next-best candidate.

## Methodology

Three stop-loss thresholds are compared: −10%, −20%, and −30%. The best result is achieved with the −10% stop: a 10,000 PLN starting capital grew to 282,200 PLN (+2,722%) with a win rate of 46.9% and max drawdown of −58.6%. Lookahead bias is avoided: signals are computed on the previous day's close, and entries execute on the next day's open. Transaction costs are not modeled explicitly in this variant; a follow-up with monthly rebalancing and Belka tax is in `technical-monthly/`.

## Data

- 354 GPW stocks, daily prices 2000–2025
- Starting capital: 10,000 PLN

## Files

- `strategia_hybrydowa_gpw.ipynb` — full daily-cadence backtest notebook
- `artykul_hybrydowa.md` — written article explaining the scoring system and stop-loss comparison
