# ML Stock Prediction — Can Technical Indicators Predict Next-Day Returns?

## Overview

This project uses 40 years of Apple (AAPL) daily data (1980–2026, 11,365 trading sessions) to test a fundamental question: do classical technical indicators contain any predictive signal for next-day price direction? The answer, backed by statistical analysis and machine learning, is clear — they do not. The project serves as an honest empirical benchmark against the common assumption that day-trading signals work.

## Methodology

Features are computed from standard indicators available on any trading platform: RSI(14), Stochastic %K and %D (14,3,3), EMA(20), MACD line/signal/histogram, Williams %R(14), and previous-day close return. The model is XGBoost with hyperparameter tuning via chronological cross-validation. The train/test split is chronological (90%/10%): trained on data up to 2021, tested on 2021–2026. Autocorrelation analysis of daily returns (ACF/PACF) is performed first, showing no statistically significant lags — a Durbin-Watson statistic near 2.0 confirms no serial correlation in returns, only in volatility.

## Key Finding

No lag from 1 to 40 days shows a statistically significant autocorrelation. The ML model fails to beat a naive baseline. Past prices and standard technical indicators do not predict tomorrow's direction. Volatility is the only predictable component — *how large* a move will be, not *which direction*.

## Files

- `01_data.ipynb` — data download, feature engineering, and autocorrelation analysis
- `02_eda.ipynb` — exploratory data analysis: return distributions, volatility regimes, indicator behavior
- `03_models.ipynb` — XGBoost model training, validation, and interpretation
- `ml_one_stock.md` — written article summarizing the findings
