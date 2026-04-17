# Intraday Strategies — Warsaw Stock Exchange (GPW)

## Overview

This project backtests four intraday trading strategies on approximately 140 stocks from the WIG20, mWIG40, and sWIG80 indices (as of March 2026), using hourly OHLCV data for the last 30 trading days sourced from Yahoo Finance. The goal is to assess whether short-term price patterns produce consistent edge on the Polish market, where liquidity is significantly lower than on major global exchanges.

## Strategies

1. **First-Hour Momentum** — rank stocks by first-hour return, buy top 3 at open of the second hour; exit on first close below the previous close, or end of day.
2. **Mean Reversion** — buy the 3 biggest first-hour losers, expecting a rebound; exit at end of day.
3. **Gap Up** — buy stocks that open more than 0.5% above the previous close; enter at the gap open, exit at end of day.
4. **End-of-Day Momentum** — buy stocks with strong end-of-session momentum; details in notebook.

## Methodology

All strategies share common assumptions: 0.05% transaction cost per side (0.1% round-trip), 100,000 PLN starting capital, equal capital split across selected stocks per day. Lookahead bias is avoided by using `Close[0]` for signal generation and `Open[1]` for entry — a distinction that in prior testing changed results from +90% to −3%.

## Files

- `intraday_strategies.ipynb` — full 4-strategy backtest notebook
- `artykul_intraday.md` — written article comparing strategies and discussing GPW liquidity constraints
