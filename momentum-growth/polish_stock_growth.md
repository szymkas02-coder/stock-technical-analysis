# I Tested a Momentum Strategy on the Warsaw Stock Exchange for 10 Years. 1,000 PLN Became 13,000 PLN.

*Reading time: ~6 min*

---

## The Idea

The premise sounds compelling: buy stocks that have been going up, sell those that are slowing down. Momentum works — at least according to the academic literature. I decided to test whether it works on the Warsaw Stock Exchange (GPW) over the period 2015–2025.

I used Python and data for 354 GPW-listed stocks. Code and notebook available on GitHub.

---

## How Does the Strategy Work?

Every month I score each stock from 0 to 6 points based on three return horizons:

- **1 month** (weight: 3 pts) — current momentum
- **12 months** (weight: 2 pts) — annual trend
- **36 months** (weight: 1 pt) — long-term fundamental growth

Points are allocated linearly and proportionally within each horizon. The final score is converted to a percentage (0–100%). The top 10 stocks enter the portfolio.

No black box — anyone can replicate this.

---

## Three Variants

I tested three approaches to see how portfolio management style affects results — independent of the scoring itself.

| | S1: Monthly rebalance | S2: Minimal rebalance | S3: Wide corridor |
|---|---|---|---|
| Entry threshold | 99% | 99% | 70% |
| Exit threshold | 90% | 90% | 30% |
| Rebalancing | Every month | Only on ranking change | Only on ranking change |

---

## Results

*(insert chart `01_kapital_porownanie.png`)*

*(insert table `05_tabela_podsumowanie.png`)*

All three strategies made money. But the differences are enormous.

**S2 — minimal rebalance** was unrivaled: from 1,000 PLN to **13,355 PLN**, a return of +1,235% over 10 years. Interestingly, it paid **6,595 PLN in tax** along the way. Yet it dominated because the gains were simply large enough that the tax drag did not matter.

**S3 — wide corridor** returned +472% with only 200 transactions and 1,356 PLN in tax. Much easier to manage, solid result — but more than 2.5× worse than S2. Fewer rotations does not mean better performance.

**S1 — monthly rebalance** performed worst: +290% with 1,646 transactions. Constantly equalizing position weights forced selling winners — this is precisely where rebalancing hurt.

---

## The Main Lesson: Don't Interfere With Winners

This was the key insight for me. S2 did nothing special — it simply **did not sell stocks that were still going up**. An exit threshold of 90% means a stock had to slow down noticeably before leaving the portfolio. The best positions were thus free to grow for years.

S1 sold a portion of its winners every month to equalize weights — and bought the laggards. The classic rebalancing trap in a momentum strategy.

An interesting paradox appears in drawdowns: S3 with the widest corridor had the largest max drawdown (−32.5%), while S1 and S2 stayed around −25%. Fewer transactions also meant a slower reaction to market deterioration.

---

## What's Next?

The model has several simplifications worth keeping in mind:

- **No transaction costs** — brokerage commissions (0.2–0.4%) would reduce results, especially for S1 with 1,646 transactions.
- **Survivorship bias** — data may not include companies that went bankrupt or were delisted. This inflates results.
- **Look-ahead bias** — scoring and the transaction both use the same month-end closing price. In practice the signal would only be available after the session closes.
- **Simplified tax** — the model deducts tax on each transaction in real time; in reality it is settled once a year in the PIT-38 tax return.

Directions for further exploration: results in an IKE/IKZE account (no Belka tax), impact of transaction costs on S1, replication on Western markets.

---

## Summary

Momentum on GPW worked — in the right form. The key was not interfering with stocks that were rising. S2, which simply held positions until they clearly slowed down, grew more than 13× in 10 years — even after paying 6,600 PLN in tax.

Code available on GitHub. If you have ideas for improving the model or want to see an IKE version — leave a comment.

---

*Analysis and code: Python. Not investment advice.*
