# Day Trading Makes No Sense — 40 Years of Apple Data as Proof

Millions of people look at RSI, MACD, and Stochastic every day to decide whether to buy or sell. I took 40 years of Apple data and empirically tested whether these indicators say anything at all about tomorrow's price.

The answer is unambiguous — and has concrete consequences for anyone thinking about day trading.

---

## Data and Methodology

**Data:** AAPL from Yahoo Finance, period 1980–2026, 11,365 trading sessions.

**Features:** RSI(14), Stochastic %K and %D (14,3,3), EMA(20), MACD line/signal/histogram, Williams %R(14), previous-day closing price. The standard set of indicators found on any trading platform.

**Question:** knowing these values from day t, can you predict the price move on day t+1 better than random chance?

Honest validation: chronological 90%/10% split, model trained on data up to 2021, tested on 2021–2026. Model: XGBoost with hyperparameter tuning via cross-validation.

[CHART: plot1_price_returns.png]

---

## Before Running Any Models — Return Autocorrelation

First analysis, before training anything: autocorrelation of daily returns.

[CHART: plot3_acf_pacf.png]

Each bar in the ACF chart is the correlation between today's return and the return from N days ago. The blue band is the 95% confidence interval — if a bar is inside the band, there is no statistically significant signal.

For no lag from 1 to 40 days does the signal exceed the confidence band. The Durbin-Watson statistic is ~2.0, consistent with zero autocorrelation.

**What this means in practice:** what Apple did yesterday (and a week ago, and a month ago) tells you nothing about what it will do tomorrow. Daily returns are statistically random noise.

The only thing that *is* autocorrelated is **volatility** — a volatile day tends to be followed by another volatile day. You can predict *how large* the next move will be, but not *in which direction*.

[CHART: plot4_volatility.png]

---

## Model Results

### Regression: What Percentage Will the Price Change Tomorrow?

| | R² |
|---|---|
| XGBoost (training) | 0.013 |
| XGBoost (holdout 2021–2026) | **0.006** |
| Dummy baseline (always predict zero) | −0.001 |

Holdout R² is 0.006 — the model explains **0.6% of the variance** in daily returns. RMSE is 1.76%, while the mean absolute daily price change is similar — the model is equivalent to predicting zero every day.

[CHART: plot6_regression_holdout.png]

### Classification: Up or Down?

| | Accuracy |
|---|---|
| XGBoost (training) | 53.8% |
| XGBoost (holdout 2021–2026) | **53.3%** |
| Dummy baseline (always predict "up") | 53.0% |

The classification model achieves 53.3% accuracy. A dummy classifier that simply always predicts "up" — because statistically there are slightly more up days — achieves 53.0%. **Model advantage over chance: 0.3 percentage points.**

[CHART: plot7_classification_results.png]

---

## Why Don't Technical Indicators Work?

RSI is a function of the last 14 closes. MACD is the difference between two EMAs. Stochastic is normalized min/max from the last N days. All these indicators are **mathematical transformations of the same closing price** — they add no new information.

And the price itself, as the ACF shows, has no memory at the one-day horizon. There is nothing to extract.

---

## What This Means for Day Trading

Consider an optimistic scenario: our model has 53.3% accuracy. With typical transaction costs (commission + spread, roughly 0.2–0.3% per trade), to break even at daily trading you need accuracy meaningfully above 50%. Even before accounting for capital gains tax (19%), this 0.3% advantage over the dummy is most likely statistical noise on 1,137 days.

Empirical research confirms this: studies of retail day traders in the Brazilian market (Chague et al., 2020) and US market (Barber & Odean) show that roughly 70–80% lose money after transaction costs.

**Important caveat:** this result applies to a specific situation — classical technical indicators, daily data, a large liquid stock like AAPL. Professional HFT traders operate on second-level data, watch order flow, and have technological advantages unavailable to retail investors. That is a completely different world from RSI on a daily chart.

---

## Conclusions

- Daily AAPL returns are statistically consistent with a random walk (EMH, weak form).
- XGBoost trained on 9 classical technical indicators does not beat a trivial baseline on 5 years of unseen data.
- Volatility is partially predictable — direction is not.
- Day trading based on technical indicators and daily data means operating on noise, not signal — transaction costs do the rest.

> A good result in machine learning does not always mean the problem is solved. Sometimes it means there is no signal here to extract.

---

*Data: Yahoo Finance. Code: Python, XGBoost, TA-Lib. Holdout: August 2021 – March 2026. Code available on GitHub.*
