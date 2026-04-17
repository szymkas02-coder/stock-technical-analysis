# I Invested 10,000 PLN in a Stock Algorithm. After 25 Years I Had 282,000 PLN.

*Reading time: ~7 min*

---

## Where Did This Come From?

Most stock trading algorithms you see online are either too simple (buy and hold) or too complex (ML models with dozens of parameters). I wanted to test something in between — classical technical indicators, a portfolio of 10 stocks, and one key question: **how wide should the stop-loss be?**

Data: 354 GPW-listed stocks, daily prices from 2000 to 2025. Python code, no paid libraries.

---

## How Does the Algorithm Work?

Every day each stock receives a composite score from 0 to 1, calculated from seven classical technical indicators:

- **RSI(14)** and **MACD** — the strongest momentum signal (40% combined weight)
- **ROC(10)** — rate of price change (20%)
- **Stochastic %K** and **CCI** — overbought/oversold oscillators (20%)
- **EMA20 vs EMA50** — trend confirmation (20%)

The portfolio holds the **top 10 scored stocks** at any time. When a stock falls below the stop-loss threshold from its purchase price — it exits, and its slot is filled by the next-best candidate.

No black box. Anyone can replicate this.

---

## Three Stop-Loss Variants

I tested three exit thresholds:

| | Stop −10% | Stop −20% | Stop −30% |
|---|---|---|---|
| Terminal capital | **282,200 PLN** | 215,777 PLN | 168,130 PLN |
| Return | **+2,722%** | +2,058% | +1,581% |
| Max Drawdown | −58.6% | −69.8% | −74.0% |
| Transactions (BUY) | 129 | 83 | 83 |
| Win Rate | **46.9%** | 42.1% | 42.0% |

---

## Results

*(insert chart `h01_kapital_porownanie.png`)*

10,000 PLN invested in 2000 grew to **282,200 PLN** with the −10% stop-loss variant. All three variants multiplied capital significantly — but the differences between them are substantial.

**Stop −10% won on every metric** — highest terminal value, highest win rate, and paradoxically the lowest max drawdown of the three. A tighter stop-loss protects capital faster and enables earlier rotation into better stocks.

**Stop −30% performed worst despite the fewest transactions.** A wide threshold means the portfolio "sits" in losing positions longer, sustaining more damage before reacting. Fewer transactions does not mean better results — a conclusion that repeats across all my tests.

---

## Year by Year

*(insert chart `h04_zwrot_roczny.png`)*

The algorithm was uneven over time. Years 2000–2003 (dot-com bust) and 2008–2009 (financial crisis) brought losses across all variants. 2008 was especially painful — Stop −30% lost 55% of its value in a single year.

But in growth years the algorithm compensated with room to spare: 2004 (+89% for Stop −10%), 2007 (+78%), 2012–2013 (+76%). Years 2020–2025 produced a streak of gains exceeding 40% per year.

---

## Drawdown — The Cost of an Aggressive Strategy

*(insert chart `h02_drawdown.png`)*

This is the biggest caveat for these results. A max drawdown of −58.6% at Stop −10% means that at the worst point the portfolio was worth more than half less than at its peak. At Stop −30% it was −74%.

Could you emotionally sustain that? I doubt most investors would stick to the algorithm through the 2008–2009 trough while watching the portfolio lose 60% of its value. This is the gap between a backtest and reality — the numbers are real, but investor psychology is not.

---

## What These Results Mean — and What They Don't

**Important caveats:**

No transaction costs — with 129 BUY transactions over 25 years this is not a major issue (commissions would reduce results marginally), but worth noting.

Survivorship bias — data may not include companies that went bankrupt or were delisted from GPW during this period. This inflates results; how much is hard to say.

Look-ahead bias — scoring and the purchase both use the same day's price. In practice the signal would only be available after market close, with the purchase executing the next morning at a different price.

No Belka tax — in reality every sale with a gain triggers 19% capital gains tax. Over 25 years with massive capital growth this would significantly reduce results.

**Price data without dividends** — `open_prices.csv` likely contains prices without reinvested dividends. Dividend-paying stocks (banks, PZU, KGHM) are therefore systematically undervalued in the score. Real total return would be higher.

---

## Summary

A hybrid algorithm combining technical scoring with a 10-stock portfolio and a −10% stop-loss produced +2,722% over 25 years on GPW. That is impressive — but before drawing investment conclusions, keep all the limitations above in mind.

Code available on GitHub. The next step I am interested in: how these results look with Belka tax and transaction costs built directly into the model.

---

*Analysis and code: Python. Not investment advice.*
