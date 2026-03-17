# Ajtoken2-minikit
Standalone tools and scripts from the AJToken2 toolkit — lightweight companions to the full AJToken2 desktop application.

**© 2025-2026 Ajumbledbox. All rights reserved.**

A collection of standalone tools and scripts from the AJToken2 toolkit.
Lightweight companions to the full AJToken2 desktop application — no
installation required, just download and run.

> **⚠ Important Notice:** These tools are designed to assist with Canadian
> cryptocurrency tax reporting but **accuracy is not guaranteed**.
> Cryptocurrency tax rules are complex and subject to change. All output
> must be independently verified before filing with the CRA. These tools
> are not a substitute for professional tax advice. **You are solely
> responsible for the accuracy of your tax filings.**

> **Looking for the full application?**
> The complete AJToken2 desktop app with full CRA tax reporting, ACB
> calculations, lot tracking, and portfolio management is available at:
> **[github.com/Ajumbledbox/ajtoken2](https://github.com/Ajumbledbox/ajtoken2)**

---

## Tools Available

### AJToken2 CRA Tax Packet Generator

Reads your Coinbase CSV export and produces CRA-compliant output files
for your Canadian tax return. Calculates Schedule 3 / T1 line values
including proceeds, adjusted cost base, and taxable capital gains using
FIFO (First In, First Out) lot accounting.

**No Python required — download the exe and run it directly.**

**What It Produces**
- `cra_packet_<year>.html` — Schedule 3 / T1 summary with CRA line values and other-income guidance
- `tax_detail_<year>.csv` — Transaction-by-transaction math trail
- `line_breakdown_<year>.csv` — Per-disposition breakdown mapped to CRA lines
- `other_income_<year>.csv` — Proof for Schedule 4 line 12100 (staking, airdrops, rewards — only if entries exist)

**CRA Lines Calculated**
- **Line 13199** — Proceeds of disposition
- **Line 13200** — Adjusted cost base (cost removed on disposal)
- **Line 13201** — Outlays and expenses
- **Line 13299** — Net capital gain/loss
- **Line 12700** — Taxable capital gains (50% inclusion rate)

**Quick Start**

Requirements: None if using the exe. Python 3.8+ if running the script directly (standard library only, no pip installs).

| Command | What You Get |
|---|---|
| `Ajtoken2.SingleScript.exe --csv my_export.csv --year 2025` | CRA packet for 2025 tax year |
| `python Ajtoken2.SingleScript.py --csv my_export.csv --year 2025` | Same, running from Python source |

**Important:** Before running, you must correctly identify all internal
wallet-to-wallet transfers in your CSV. Sends between your own wallets
are not dispositions — if they are not flagged as internal transfers,
the tool will treat them as sales and produce incorrect capital gains.

**Supported Transaction Types**

| Type | Classification |
|---|---|
| Buy, Advanced Trade Buy | Acquisition |
| Sell, Advanced Trade Sell | Disposition |
| Receive | Acquisition (at fair market value) |
| Send | Disposition (unless flagged as internal transfer) |
| Convert | Acquisition or Disposition (based on direction) |
| Staking Income, Reward Income | Acquisition |
| Deposit | Acquisition |

---

## Coming Soon

### AJToken2 Current Pricing Tool

Reads your Coinbase CSV export and traces original purchase dollars (CAD)
through every transaction using FIFO lot tracking. Shows your current
portfolio value using live prices from CoinMarketCap with realized and
unrealized P/L in CAD.

**Key Features**
- **FIFO Lot Tracking** — Every buy creates a lot. Every sell consumes the oldest lots first. Remaining lots show exactly what you paid (fee-inclusive CAD)
- **Live Portfolio Pricing** — Current CAD prices from CoinMarketCap for all held tokens
- **Realized & Unrealized P/L** — Gains/losses on completed sells and current holdings side by side
- **Total P/L** — One number answering "across everything I've bought and sold, am I up or down?"
- **USDC Break-even** — CAD-adjusted price target to recover your full CAD investment at today's exchange rate
- **Fee-Adjusted Break-even** — Break-even accounting for Coinbase Advanced Trade's 0.6% base fee
- **CSV Comparison Mode** — Process two CSV exports from different time periods with delta analysis
- **Excel Workbook Output** — Multi-sheet .xlsx with summary, per-token lot sheets, and conditional formatting (green/red P/L)

**Output Columns**

| Column | Description |
|---|---|
| Asset | Token symbol |
| Remaining Qty | Tokens still held |
| Purchase Value (CAD) | Original fee-inclusive CAD paid for held tokens |
| Curr.$.CAD | Live CAD price from CoinMarketCap |
| Mrkt.V.CAD | Remaining Qty × Current Price |
| Curr.P/L | Market Value − Purchase Value |
| Real.P/L | Cumulative gain/loss from all completed sells |
| Total P/L | Complete picture combining held value and realized gains/losses |
| Total USDC Cost | Aggregate USDC spent on remaining holdings |
| USDC.BRKevn.$ | CAD-adjusted USDC break-even price (pre-fee) |
| BrkEvn+0.6%Fee | Break-even adjusted for Coinbase 0.6% fee |
| Curr.USDC.$ | Current token price in USDC terms for comparison |

A **Portfolio Total** row sums key columns across all tokens.

**Quick Start**

Requirements: Python 3.8+, `openpyxl` for Excel output (`pip install openpyxl`), internet connection for live prices.

| Command | What You Get |
|---|---|
| `python ajtokenCurrentPricing.py` | Single token (ETH default) — lot-by-lot breakdown |
| `python ajtokenCurrentPricing.py --asset DOGE` | Single token (DOGE) — lot-by-lot breakdown |
| `python ajtokenCurrentPricing.py --all` | All tokens — summary table with live prices and P/L |
| `python ajtokenCurrentPricing.py --all --workbook lots.xlsx` | All tokens — terminal + multi-sheet Excel workbook |
| `python ajtokenCurrentPricing.py --all --output summary.csv` | All tokens — terminal + CSV export |
| `python ajtokenCurrentPricing.py --csv myfile.csv --all` | Use a custom CSV file |

**Comparison Mode** — Compare two CSV exports from different time periods:

```bash
python ajtokenCurrentPricing.py --csv jan_export.csv --csv2 mar_export.csv --all
python ajtokenCurrentPricing.py --csv jan_export.csv --csv2 mar_export.csv --all --workbook comparison.xlsx
```

---

## How FIFO Lot Tracking Works

Both tools use the same FIFO methodology:

1. Reads all transactions from your Coinbase CSV, sorted chronologically
2. Each buy (including receives, converts, staking rewards) creates a **lot** with original quantity and fee-inclusive CAD cost
3. Each sell (including sends, negative converts) **consumes** the oldest lot first (FIFO)
4. Remaining lots represent unsold inventory with original purchase dollars preserved
5. USDC is tracked with full FIFO lot tracking using synthetic counterparty transactions
6. CAD uses a running-balance method

---

## Important Disclaimers

These tools are provided **"AS IS"** without warranty of any kind, express
or implied. They do not constitute financial, tax, or legal advice. Users
are solely responsible for verifying all output before filing with the CRA.

Licensed for personal, non-commercial use only. Redistribution, resale,
or incorporation into commercial products is prohibited without explicit
written permission from Ajumbledbox.

---

## Support This Project

If you find these tools useful consider supporting development.

### Fiat Donations
[![PayPal](https://img.shields.io/badge/Donate-PayPal-blue)](https://paypal.me/thetinkeringtoad)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-☕-blue)](https://ko-fi.com/ajumbledbox)

**Support via Stripe — quick and easy, no account needed:**

| Amount | Link |
|---|---|
| ☕ Tip $2 | [Buy me a coffee](https://buy.stripe.com/cNi9AMf71fDjdfq7xe7wA00) |
| 🍕 Snacks $5 | [Buy me a snack](https://buy.stripe.com/bJe4gsaQLfDj3EQ6ta7wA01) |
| 💪 Support $25 | [Support the projects](https://buy.stripe.com/8x26oA2kf0Ip1wI2cU7wA02) |

### Crypto Donations
All crypto donations go directly to a personal wallet — no middleman, no fees.

| Asset | Network | Address |
|---|---|---|
| BTC | Bitcoin | `bc1q8tct3jc3v7ankrwsscl4ne6u7vx7uvd6yrc06f` |
| ETH | Ethereum | `0xA084AdDe9D97fc583B4ACc4Da6a132B36a563eCe` |
| SOL | Solana | `6eYC7zSaEpE6W15FWJwhnBqwxUbF8EKPEmHGzqkzroG8` |
| XRP | XRP Ledger | `rJ7xCRV5fJv7fhbHaMUtAFBDwee3k3YRDc` |
| USDC | Solana | `6eYC7zSaEpE6W15FWJwhnBqwxUbF8EKPEmHGzqkzroG8` |
| DOGE | Dogecoin | `DNWe2Xx89fsdBbXSjGFEj3cTjUGeJUzpiC` |
| LTC | Litecoin | `LMb1gPvkeFvoLznGTnA6fT3usHPABDnNcd` |

---

**© 2025-2026 Ajumbledbox. All rights reserved.**
