# Stock Technical Analysis

Quantitative research notebooks covering momentum and technical indicator strategies on the Warsaw Stock Exchange (GPW), intraday strategies for both Polish equities and cryptocurrencies, and a machine learning pipeline for single-stock price prediction. All backtests use real historical data with transaction costs and Polish capital gains tax (Belka, 19%) applied where relevant.

## Subprojects

| Folder | Description |
|---|---|
| [momentum-growth/](momentum-growth/) | Multi-horizon momentum scoring strategy on 354 GPW stocks (2015–2025), converting 1,000 PLN into 13,000 PLN over 10 years |
| [hybrid-strategy/](hybrid-strategy/) | Hybrid technical indicator strategy with daily stop-loss on 354 GPW stocks (2000–2025), testing three stop-loss thresholds |
| [technical-monthly/](technical-monthly/) | Same technical scoring on monthly rebalancing cadence with per-transaction Belka tax applied in the model |
| [intraday-gpw/](intraday-gpw/) | Four intraday strategies (momentum, mean reversion, gap up, end-of-day) on ~140 WSE stocks from WIG20/mWIG40/sWIG80 |
| [intraday-crypto/](intraday-crypto/) | Eight intraday strategies on the top 20 cryptocurrencies by market cap, using hourly data |
| [ml-stock-prediction/](ml-stock-prediction/) | XGBoost-based ML pipeline on 40 years of Apple (AAPL) data to test whether technical indicators predict next-day returns |

## Technologies

- Python, Jupyter Notebooks
- `pandas`, `numpy`, `matplotlib`, `yfinance`
- `scikit-learn`, `xgboost`
- Technical indicators: RSI, MACD, Stochastic, EMA, CCI, ROC, Williams %R
