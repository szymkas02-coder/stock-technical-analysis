# 8 Intraday Strategies on Cryptocurrencies — Which One Actually Works?

I tested 8 intraday strategies on the top 20 cryptocurrencies. The methodology is the same as in the previous GPW project — no shortcuts, no data cheating. The results are surprising.

---

## The Idea

In the previous project I tested intraday strategies on Polish-listed stocks. The natural next question: do the same mechanisms work on cryptocurrencies? Crypto is a completely different market — it trades 24/7, is far more volatile, and retail traders dominate over institutions. I decided to test this empirically.

But I did not want to test strategies only on Bitcoin. That is too narrow a slice of the market. Instead I took a portfolio approach: **every day I rank all 20 cryptocurrencies** by the signal of a given strategy and invest equally in the **top 3** best candidates — exactly as with the equity strategies.

---

## Data and Methodology

**Data:** Yahoo Finance, 1-hour interval, last 30 days, top 20 cryptocurrencies by market capitalization (Bitcoin, Ethereum, BNB, Solana, XRP, Cardano, Avalanche, Dogecoin, TRON, Polkadot, Chainlink, Polygon, Litecoin, Uniswap, Cosmos, Stellar, NEAR, Internet Computer, Filecoin, Algorand).

**Starting capital:** $10,000

**Transaction costs:** 0.1% per side (0.2% round-trip) — typical for Binance or Coinbase.

**Critical rule (no lookahead bias):**
```
Signal  → based on data from the close of candle [0]
Entry   → Open[1], the first price after signal confirmation
```
This seemingly small detail has enormous consequences. In the previous project, ignoring this rule (buying at Open[0] instead of Open[1]) changed the result from +90% to −3%. Hence strict enforcement.

[CHART: crypto_plot1_equity_curves.png]

---

## 8 Strategies — Their Origins

I tested two groups of strategies:

**Carried over from the GPW project** (adapted for 24/7 crypto):
1. First-hour Momentum
2. First-hour Mean Reversion
3. Gap Up (gap vs previous close)
4. EoD Streak (consecutive rising candles)

**New — crypto-specific:**
5. RSI Oversold
6. Bollinger Bands
7. EMA Golden Cross
8. MACD + RSI Hybrid

---

## How the Strategies Work

### 1. Momentum 1h
Each day we compute the first-hour return `(Close[0] - Open[0]) / Open[0]` for each of the 20 cryptocurrencies. Buy the top 3 with the strongest momentum at the open of the second candle. Exit when price starts falling (trailing stop) or at end of day.

### 2. Mean Reversion 1h
The opposite — buy the bottom 3, i.e. cryptos that fell most in the first hour. The bet: large short-term moves are exaggerated and price will revert to the mean. Exit EOD.

### 3. Gap Up
Look for cryptos that opened more than 0.5% above the previous close. Buy the top 3 ranked by gap size. Exit EOD.

### 4. EoD Streak
Count how many consecutive candles in a row a given crypto has been rising (across the full available hourly history, ending at Close[0]). Top 3 with the longest streak — bet on trend continuation. Exit EOD.

### 5. RSI Oversold
RSI(14) computed on the full hourly history. A crypto qualifies if RSI is below 30 at Close[0] — classic oversold signal. Rank by lowest RSI, buy top 3. Exit EOD.

### 6. Bollinger Bands
Bollinger Bands (20-candle window, 2 standard deviations). Signal when Close[0] falls below the lower band. Rank by distance below the band (further below = higher priority). Exit EOD.

### 7. EMA Golden Cross
Look for cryptos where EMA(8) has just crossed above EMA(21) within the last 3 candles. Rank by strength of separation between the lines. Classic trend signal. Exit EOD.

### 8. MACD + RSI Hybrid
Signal only when two conditions are simultaneously met: MACD > MACD signal line **and** RSI < 40. Rank by combined signal strength: `(MACD - Signal) × (40 - RSI)`. Exit EOD.

---

## Results

| Strategy | Return | Max DD | Sharpe | Win Rate | Transactions |
|---|---|---|---|---|---|
| 1. Momentum | −2.29% | −6.22% | −1.61 | 30.2% | 96 |
| 2. Mean Reversion | −16.83% | −15.39% | −2.49 | 34.4% | 96 |
| 3. Gap Up | none | — | — | — | 0 |
| 4. EoD Streak | −14.40% | −4.42% | −3.55 | 52.1% | 48 |
| 5. RSI Oversold | −14.25% | −12.93% | −4.11 | 30.6% | 36 |
| 6. Bollinger Bands | −3.48% | −3.22% | −6.58 | 12.5% | 8 |
| 7. **EMA Golden Cross** | **+21.79%** | **−2.01%** | **7.09** | **52.2%** | 23 |
| 8. MACD + RSI | −2.69% | −5.24% | −1.95 | 28.6% | 14 |

[CHART: crypto_plot4_win_rate.png]

---

## What Do These Numbers Tell Us?

### EMA Golden Cross — The Only Clear Winner

+21.79% in 30 days with a max drawdown of just −2.01% and Sharpe 7.09 — a very strong result. The strategy generated only 23 transactions, reflecting selectivity: it enters rarely but accurately. Top cryptos: NEAR (+16.4%), Chainlink (+6%), Cardano (+3.7%).

The intuition is simple: an EMA(8/21) golden cross on the hourly chart means short-term momentum has just turned upward. In the crypto environment, where trends can be violent, this signal carries real information.

Caution is warranted, however — 23 transactions is a small sample. The result may be partly luck or a coincidentally favorable time window.

[CHART: crypto_plot2_drawdown.png]

### Mean Reversion — The Worst Result

−16.83% and a win rate of 34.4%. Mean Reversion works contrary to intuition here: a crypto that drops sharply in the first hour... often continues to drop. In crypto markets, momentum is more powerful than mean reversion. Falling crypto attracts more sellers, not buyers.

This is the exact opposite of the GPW result, where Mean Reversion was the only strategy that finished positive (+5.33%). Equity and crypto markets behave very differently.

### RSI and Bollinger Bands — Classic Indicator Traps

RSI Oversold: −14.25%, win rate 30.6%. Bollinger Bands: −3.48%, win rate 12.5%. Both indicators were designed for markets with limited volatility. In crypto, RSI < 30 does not mean something is "cheap" — it may mean a crash has just begun. The lower Bollinger Band is not a floor, just an observation that price is low relative to the last 20 candles.

### Gap Up — Zero Transactions

The Gap Up strategy generated no transactions at all. The 0.5% gap threshold was either too high, or cryptos in the test window simply did not form significant hourly gaps. A 24/7 market has no "close" in the traditional sense — cryptocurrencies trade continuously, so overnight gaps barely exist.

[CHART: crypto_plot3_distributions.png]

### EoD Streak — High Win Rate, Massive Loss

An interesting case: win rate 52.1% (most trades positive) but total return −14.40%. Why? The return distribution is asymmetric — small gains and occasional catastrophic losses. Worst single trade: −15.27%. In crypto, long winning streaks often end with a violent reversal.

[CHART: crypto_plot5_top_tickers.png]

---

## Comparison with GPW Results

| Strategy | GPW (30 days) | Crypto (30 days) |
|---|---|---|
| Momentum | −3.23% | −2.29% |
| Mean Reversion | **+5.33%** | −16.83% |
| Gap Up | −17.67% | no transactions |
| EoD Streak | −2.98% | −14.40% |

Momentum delivers similarly weak results on both markets. Mean Reversion is the best strategy on GPW and the worst on crypto. Gap Up is catastrophic on GPW and useless on crypto. This demonstrates that the same mechanisms behave completely differently depending on the market.

---

## Limitations and Caveats

**Short test period.** 30 days is insufficient for strong conclusions. EMA Golden Cross results may be specific to the particular month when NEAR experienced an exceptionally strong move.

**Yahoo Finance data.** Hourly crypto data from Yahoo Finance may contain gaps and inaccuracies, especially for less liquid tokens.

**No slippage.** The model assumes execution at the exact Open[1] price. In reality, for larger orders the fill price will be worse.

**Taxes and regulation.** Crypto trading is subject to taxation. In Poland, crypto gains are taxed at 19% capital gains tax (Belka). Frequent intraday trades generate many tax events.

**Not investment advice.** Past performance does not guarantee future results. Investing in cryptocurrencies involves a high risk of capital loss.

---

## Code

Full code available in the repository: Python notebook with data download, implementation of all 8 strategies, and visualizations.
