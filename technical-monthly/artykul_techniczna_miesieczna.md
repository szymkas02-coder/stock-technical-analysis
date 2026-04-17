# The Algorithm Turned 10,000 PLN into 500,000 PLN. Without Tax It Would Have Been 2 Million.

*Reading time: ~7 min*

---

## The Question That Troubled Me

In the previous test, the hybrid strategy with a daily stop-loss delivered +2,722% over 25 years — but without Belka tax. Would the same approach work with a monthly decision cycle and tax applied directly in the model?

Data: the same — 354 GPW stocks, 2000–2025. Starting capital: 10,000 PLN. This time, however, every sale with a gain immediately deducts 19%.

---

## How Does This Model Work?

The technical scoring is identical — RSI, MACD, Stochastic, CCI, ROC, and the EMA20/EMA50 relationship, combined into a score of 0–100% for each stock. The change is in the decision rhythm: instead of reacting daily to a stop-loss, the portfolio is reviewed **once a month**.

A stock leaves the portfolio when its score drops below the exit threshold. New positions enter when the score exceeds the entry threshold. The portfolio holds up to 10 stocks simultaneously.

Three variants differ in the width of the "corridor":

| | S1 Conservative | S2 Standard | S3 Aggressive |
|---|---|---|---|
| Entry threshold | 80% | 60% | 40% |
| Exit threshold | 40% | 30% | 20% |

---

## Results

*(insert chart `m01_kapital_porownanie.png`)*
*(insert table `m07_tabela.png`)*

All three strategies multiplied capital significantly. But the ranking was surprising.

**S3 Aggressive won with +4,997%** — 10,000 PLN became 509,673 PLN despite paying 136,387 PLN in tax. The low entry threshold (40%) means the portfolio is almost always fully invested, and the low exit threshold (20%) gives stocks plenty of room before they are removed. Only 211 transactions over 25 years.

**S1 Conservative came second with +3,222%** — 332,182 PLN terminal capital, but at the cost of 1,025 transactions and 228,095 PLN in tax. The high entry threshold (80%) makes the portfolio selective — which ironically generates more frequent rotation and a higher tax bill.

**S2 Standard performed worst with +2,285%** — 238,523 PLN. Middle thresholds provide neither the protection of the conservative approach nor the freedom of the aggressive one.

---

## The Main Lesson: Belka Tax Can Take 85% of Your Gains

*(insert chart `m04_brutto_vs_netto.png`)*

This is the chart that makes you stop.

S1 without tax would have earned **2,168,667 PLN**. With tax — 332,182 PLN. Tax took **1,836,485 PLN** — over 84% of all generated profit.

Why such a difference? S1 executed 1,025 transactions — an average of more than 3 per month over 25 years. Every profitable sale sends 19% to the tax authority immediately. Gains that could have compounded for years are instead realized and taxed continuously.

S3 with 211 transactions paid "only" 136,387 PLN in tax — six times less than S1, while producing a better final result. Fewer decisions = less frequently realized gains = less frequently paid tax.

---

## Year by Year

*(insert chart `m05_zwrot_roczny.png`)*

The strategy is uneven over time. Years 2000–2001 brought losses (dot-com bust). Year 2008 was painful — S1 and S2 lost more than 60% in a single year.

But growth years compensate with room to spare: S1 in 2002 +254%, in 2020 +362%. S3 in 2021 +214%. A handful of exceptional years drives the entire result — which is both the strength and the weakness of momentum strategies.

---

## Drawdown — The Price of This Strategy

*(insert chart `m02_drawdown.png`)*

S1's max drawdown is −80.2%. At the worst point the portfolio was worth one-fifth of its peak value. Especially striking is the 2008–2020 period for S1 — for more than a decade the strategy never returned to its previous highs. Who could psychologically endure that?

S3 paradoxically handles this best — max drawdown "only" −73.2%, and it recovers to new highs faster thanks to the wide corridor that does not eject stocks on every temporary dip.

---

## Limitations

- **Survivorship bias** — companies that went bankrupt may not be in the data
- **Look-ahead bias** — scoring and transaction use the same month-end closing price
- **Simplified tax** — deducted immediately rather than annually; does not account for offsetting losses against gains
- **No transaction costs** — with 1,025 transactions, S1's brokerage commissions would further reduce results
- **Price data without dividends** — dividend-paying stocks are undervalued in the score

---

## Summary

The strategy works — but portfolio management has fundamental importance. S3 with a low threshold and infrequent transactions beat S1 with a high threshold and frequent rotation.

The most important conclusion of this analysis: with an active strategy, Belka tax can take 85% of your gains. An IKE or IKZE account stops being a "nice extra" — it becomes the difference between 332,000 and 2,000,000 PLN on the same algorithm.

That will be the subject of the next test.

Code available on GitHub.

---

*Analysis and code: Python. Not investment advice.*
