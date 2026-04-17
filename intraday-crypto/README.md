# Intraday Strategies — Cryptocurrencies

## Overview

This project extends the GPW intraday strategy framework (`intraday-gpw/`) to the top 20 cryptocurrencies by market capitalization (Bitcoin, Ethereum, BNB, Solana, XRP, Cardano, Avalanche, Dogecoin, TRON, Polkadot, Chainlink, Polygon, Litecoin, Uniswap, Cosmos, Stellar, NEAR, Internet Computer, Filecoin, Algorand). Eight strategies are tested on hourly data for the last 30 days from Yahoo Finance, using 10,000 USD starting capital and 0.1% per-side transaction costs (0.2% round-trip, typical for Binance/Coinbase).

## Strategies

Four strategies are carried over from the GPW project (adapted for 24/7 crypto trading): first-hour momentum, mean reversion, gap up, and end-of-day momentum. Four additional strategies exploit crypto-specific characteristics: volatility breakout, volume surge, overnight gap, and RSI extreme reversion.

## Methodology

The portfolio approach ranks all 20 cryptocurrencies daily by signal strength and invests equally in the top 3. The same strict no-lookahead rule applies: signal from `Close[0]`, entry at `Open[1]`. This detail matters critically — on the GPW project, ignoring it changed the result from +90% to −3%.

## Files

- `krypto_strategie.ipynb` — full 8-strategy backtest notebook
- `artykul_krypto_strategie.md` — written article comparing strategies across crypto vs. equity markets
