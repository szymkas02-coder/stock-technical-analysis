# Technical Monthly Strategy — WSE (GPW) with Per-Transaction Tax

## Overview

This project extends the hybrid technical scoring strategy from `hybrid-strategy/` by switching to a monthly decision cadence and applying Polish capital gains tax (Belka, 19%) on every profitable sale in real time, not deferred to the end. The same 354 GPW stocks, same 25-year dataset (2000–2025), same composite scoring system — but portfolio reviews happen monthly and tax is deducted from each realized gain before reinvestment.

## Methodology

Three portfolio corridor variants are tested, differing in how tightly stocks are managed: S1 Conservative (entry ≥ 80%, exit < 40%), S2 Standard (entry ≥ 60%, exit < 30%), and S3 Aggressive (entry ≥ 40%, exit < 20%). The score is read at each month-end and trades execute at the following month-end price, so there is no lookahead bias. Counter-intuitively, the aggressive low-threshold variant wins: S3 grows 10,000 PLN into 168,114 PLN (+1,581%) while generating only 218 transactions and paying 49,152 PLN in tax. S1 Conservative reaches 315,908 PLN (+3,059%) but at the cost of 1,042 transactions and 201,643 PLN in tax; S2 Standard lands in between (+1,848%, 194,795 PLN). The takeaway holds: high entry thresholds increase churn and tax burden. Returns remain gross of slippage and are subject to survivorship bias (universe = stocks listed today).

## Data

- 354 GPW stocks, daily prices 2000–2025
- Starting capital: 10,000 PLN
- Belka tax: 19% applied at each sell transaction on the realized gain

## Files

- `strategia_techniczna_miesieczna.ipynb` — monthly-cadence backtest with per-transaction tax
- `artykul_techniczna_miesieczna.md` — written article discussing the tax drag findings (kept local, not tracked by git)
