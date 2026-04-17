# Technical Monthly Strategy — WSE (GPW) with Per-Transaction Tax

## Overview

This project extends the hybrid technical scoring strategy from `hybrid-strategy/` by switching to a monthly decision cadence and applying Polish capital gains tax (Belka, 19%) on every profitable sale in real time, not deferred to the end. The same 354 GPW stocks, same 25-year dataset (2000–2025), same composite scoring system — but portfolio reviews happen monthly and tax is deducted from each realized gain before reinvestment.

## Methodology

Three portfolio corridor variants are tested, differing in how tightly stocks are managed: S1 Conservative (entry ≥ 80%, exit < 40%), S2 Standard (entry ≥ 60%, exit < 30%), and S3 Aggressive (entry ≥ 40%, exit < 20%). Counter-intuitively, the aggressive low-threshold variant wins: S3 grows 10,000 PLN into 509,673 PLN (+4,997%) while generating only 211 transactions and paying 136,387 PLN in tax. S1 Conservative, despite its selectivity, produces 1,025 transactions and 228,095 PLN in tax — demonstrating that high entry thresholds increase churn and tax burden, not reduce them.

## Data

- 354 GPW stocks, daily prices 2000–2025
- Starting capital: 10,000 PLN
- Belka tax: 19% applied at each sell transaction on the realized gain

## Files

- `strategia_techniczna_miesieczna.ipynb` — monthly-cadence backtest with per-transaction tax
- `artykul_techniczna_miesieczna.md` — written article discussing the tax drag findings
