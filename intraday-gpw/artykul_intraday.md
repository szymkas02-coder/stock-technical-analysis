# Can You Make Money Intraday on the Warsaw Stock Exchange? Testing 4 Strategies on WIG20/mWIG40/sWIG80

Intraday trading is associated with dynamic markets — Wall Street, crypto, forex. But what about GPW (Warsaw Stock Exchange)? Do short-term strategies make sense in the Polish equity market, where liquidity is incomparably lower? I decided to find out — with a backtest on hourly data from the last 30 trading days, across a broad universe of ~140 stocks from the WIG20, mWIG40, and sWIG80 indices.

---

## Test Rules

All strategies share the same set of assumptions:

- **Data:** hourly OHLCV candles from Yahoo Finance, last 30 trading sessions
- **Universe:** ~140 stocks from WIG20, mWIG40, and sWIG80 (composition as of 06.03.2026)
- **Starting capital:** 100,000 PLN
- **Transaction costs:** 0.05% per side (0.1% round-trip) — typical for Polish retail brokers
- **Capital allocation:** equal split among selected stocks on a given day
- **Critical rule:** signal is generated using the closing price of the first candle (Close[0]); entry happens only at the open of the *next* candle (Open[1]) — to avoid lookahead bias

---

## 4 Strategies

### 1. First-Hour Momentum

Ranks stocks by their return in the first trading hour `(Close[0] - Open[0]) / Open[0]`, selects the top 3, and buys them at the open of the second hour. Exit occurs on the first close below the previous close, or at end of day if the position kept rising.

**Logic:** a stock that rises sharply in the first hour has momentum — other investors will notice and join.

### 2. Mean Reversion

The opposite of momentum — selects the 3 stocks with the *largest first-hour declines*, betting that a strong down-move will be followed by a bounce. Buys at Open[1], holds until end of day.

**Logic:** on a shallow market like GPW, overreactions are frequent — a stock that falls 2% in one hour for no fundamental reason often bounces back by midday.

### 3. Gap Up

Buys stocks that opened with a gap up of more than 0.5% versus the previous close. Entry at Open[0] (because the signal comes from the previous day), exit at end of day.

**Logic:** a gap up signals a positive sentiment shift — someone was buying overnight or in the pre-market.

### 4. End-of-Day Momentum

Looks for stocks with at least 2 consecutive rising candles during the session. Enters at the closing price of the signal candle, holds until end of day.

**Logic:** a consistent intraday trend has a higher probability of continuation than a one-off impulse.

---

## Results

[CHART: plot1_equity_curves.png]

| Strategy | Return | Max DD | Sharpe | Win Rate | Transactions |
|---|---|---|---|---|---|
| **Momentum** | −3.23% | −8.85% | −1.55 | 31.7% | 63 |
| **Mean Reversion** | **+5.33%** | −5.49% | **2.64** | **55.6%** | 63 |
| **Gap Up** | −17.67% | −16.46% | −10.69 | 30.0% | 60 |
| **EoD Momentum** | −2.98% | −3.12% | −5.91 | 23.8% | 63 |

The results are telling. Of the four strategies only one finished in positive territory — **Mean Reversion** with a return of +5.33% over the month and a Sharpe ratio of 2.64. The other three produced losses.

[CHART: plot2_drawdown.png]

### What Went Wrong with Momentum?

First-hour momentum seems intuitive — follow the strong stocks. The problem is that on GPW, a strong move in the first hour often *exhausts* the day's demand. A stock that rose 3% by 10:00 a.m. is likely to consolidate or retrace for the rest of the session. Result: 31.7% win rate and −3.23% return.

[CHART: plot3_return_distributions.png]

### Why Did Gap Up Disappoint So Badly?

Gap Up performed worst of all strategies (−17.67%, Sharpe −10.69). The intuition is simple — a gap up is a good sign. In practice on GPW, gaps are frequently "filled" the same day: the market opens high but there are no further buyers, so the price drifts lower for the rest of the session. The strategy consistently bought near daily highs.

### Mean Reversion — Luck or Edge?

+5.33% in a month at Sharpe 2.64 is an impressive result — but caution is warranted. 30 days is too short a period to draw far-reaching conclusions. The mechanism is, however, logical for GPW: with low liquidity, overreactions are frequent, and large institutions often accumulate discounted shares intraday.

[CHART: plot4_win_rate_avg_return.png]

### Top Stocks per Strategy

[CHART: plot5_top_tickers.png]

Worth noting: **Captor Therapeutics** appears in both the top Momentum (+18.38%) and Mean Reversion (+2.98%) lists, suggesting it was a stock with exceptional volatility during the test period rather than a systematic strategy effect.

---

## Transaction Costs Matter

Assuming 0.1% round-trip, every transaction starts with a built-in loss. With 63 transactions per month and an average allocation of ~33,333 PLN per position, costs alone consume ~2,100 PLN per month. For strategies with small average returns (Momentum: −0.14%, EoD: −0.14%) this is the difference between profit and loss.

In practice on GPW, worth considering:
- Reducing trade frequency (top 1 instead of top 3)
- Higher signal threshold (e.g., gap > 1% instead of > 0.5%)
- Liquidity filter — skip stocks with daily turnover below 1 million PLN

---

## Summary

Short-term intraday strategies on GPW are possible — but the market rewards the *opposite* of intuition. Mean Reversion, i.e. betting on a reversal after a strong move, proves more effective than following momentum. Gap Up, which many investors treat as a buy signal, consistently generated losses in this test.

**This is not investment advice** — 30 days is too short for reliable conclusions, and backtest results rarely translate 1:1 into live trading. Treat these results as a starting point for further research, not a ready-made strategy.

Notebook code available on GitHub.

---

*Data: Yahoo Finance. Backtest implemented in Python (yfinance, pandas, matplotlib). Transaction costs 0.1% round-trip. Belka tax, price slippage, and illiquidity not modeled.*
