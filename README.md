# Ajtoken2-minikit


**© 2013-2026 Ajumbledbox. All rights reserved.**

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

**Self-contained executable — no installation required. Just download and run.**

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

**How to Run**

1. Download `Ajtoken2.SingleScript.exe` from the releases page
2. Place it in any folder on your machine
3. Double-click to run — a terminal window will open
4. Drag your Coinbase CSV file into the terminal window when prompted, or type the full path
5. Enter the tax year when prompted (e.g. 2025)
6. The tool will detect any Send transactions and ask you to confirm whether each one was a transfer to your own wallet or a payment to someone else — answer carefully as this affects your capital gains calculation
7. Output files appear in the same folder as the exe when complete

**Output Files**

| File | Contents |
|---|---|
| `cra_packet_<year>.html` | Open in any browser — your complete Schedule 3 / T1 summary with all CRA line values and a transaction-by-transaction proof table |
| `tax_detail_<year>.csv` | Full math trail of every acquisition and disposition with running ACB — use this to verify the calculations |
| `line_breakdown_<year>.csv` | Each disposition mapped to its CRA line — useful for manually cross-checking your return |
| `other_income_<year>.csv` | Staking rewards, airdrops, and Coinbase Earn entries for Schedule 4 line 12100 — only created if entries exist |

**Important:** When you run the tool it will detect every Send transaction 
in your CSV and ask you to confirm whether each one was a transfer to your 
own wallet or a payment to someone else. Answer carefully — Send transactions 
between your own wallets are not taxable disposals. Incorrect answers will 
produce wrong capital gains figures on your CRA return.

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

### AJToken2 Current Pricing Tool

Reads your Coinbase CSV export and traces original purchase dollars (CAD)
through every transaction using FIFO lot tracking. Shows your current
portfolio value using live prices from CoinMarketCap with realized and
unrealized P/L in CAD.

**Available as a standalone GUI application and as a Python script.**

**How to Run the GUI Application**

1. Download the `.exe` from the releases page
2. Place it in any folder on your machine
3. Double-click to run — a window opens
4. On first launch, read and accept the Terms of Use and Liability Agreement
5. Click Browse to select your Coinbase CSV file
6. Choose All Tokens or Single Token mode and click Run
7. Results appear in the table with live pricing

**Key Features**
- **FIFO Lot Tracking** — Every buy creates a lot. Every sell consumes the oldest lots first. Remaining lots show exactly what you paid (fee-inclusive CAD)
- **Live Portfolio Pricing** — Current CAD prices from CoinMarketCap for all held tokens
- **Realized & Unrealized P/L** — Gains/losses on completed sells and current holdings side by side
- **Total P/L** — One number answering "across everything I've bought and sold, am I up or down?"
- **USDC Break-even** — CAD-adjusted price target to recover your full CAD investment at today's exchange rate
- **Fee-Adjusted Break-even** — Break-even accounting for Coinbase Advanced Trade's 0.6% base fee
- **View Lots** — Click any asset to see individual lot detail in a popup window. Multiple lot windows can be open simultaneously.
- **CSV Comparison Mode** — Compare two CSV exports from different time periods with a 3-panel view: delta table showing what changed, plus both summaries side by side
- **Export** — Export to CSV or multi-sheet Excel workbook (.xlsx) with summary and per-token lot sheets
- **Font Size** — Adjustable font size for all tables and labels via View menu

**Output Columns**

| Column | Description |
|---|---|
| Asset | Token symbol |
| Remaining Qty | Tokens still held |
| Purchase Value (CAD) | Original fee-inclusive CAD paid for held tokens |
| Curr.$.CAD | Live CAD price from CoinMarketCap |
| Mrkt.V.CAD | Remaining Qty × Current Price |
| Curr.P/L.CAD | Market Value − Purchase Value (in CAD) |
| Curr.P/L.USDC | Market Value − Purchase Value (in USDC terms) |
| Real.P/L | Cumulative gain/loss from all completed sells |
| Total P/L | Complete picture combining held value and realized gains/losses |
| Total USDC Cost | Aggregate USDC spent on remaining holdings |
| USDC.BRKevn.$ | CAD-adjusted USDC break-even price (pre-fee) |
| BrkEvn+0.6%Fee | Break-even adjusted for Coinbase 0.6% fee |
| Curr.USDC.$ | Current token price in USDC terms for comparison |

A **Portfolio Total** row sums key columns across all tokens.

**Python Script Version**

The Current Pricing Tool is also available as a Python script for users who prefer command-line operation.

Requirements: Python 3.8+, `openpyxl` for Excel output (`pip install openpyxl`), internet connection for live prices.

| Command | What You Get |
|---|---|
| `python ajtokenCurrentPricing.py --csv myfile.csv` | Single token (ETH default) — lot-by-lot breakdown |
| `python ajtokenCurrentPricing.py --csv myfile.csv --asset DOGE` | Single token (DOGE) — lot-by-lot breakdown |
| `python ajtokenCurrentPricing.py --csv myfile.csv --all` | All tokens — summary table with live prices and P/L |
| `python ajtokenCurrentPricing.py --csv myfile.csv --all --workbook lots.xlsx` | All tokens — terminal + multi-sheet Excel workbook |
| `python ajtokenCurrentPricing.py --csv myfile.csv --all --output summary.csv` | All tokens — terminal + CSV export |

**Comparison Mode** — Compare two CSV exports from different time periods:

```bash
python ajtokenCurrentPricing.py --csv jan_export.csv --csv2 mar_export.csv --all
python ajtokenCurrentPricing.py --csv jan_export.csv --csv2 mar_export.csv --all --workbook comparison.xlsx
```

---

## How FIFO Lot Tracking Works

All tools use the same FIFO methodology:

1. Reads all transactions from your Coinbase CSV, sorted chronologically
2. Each buy (including receives, converts, staking rewards) creates a **lot** with original quantity and fee-inclusive CAD cost
3. Each sell (including sends, negative converts) **consumes** the oldest lot first (FIFO)
4. Remaining lots represent unsold inventory with original purchase dollars preserved
5. USDC is fully tracked with full FIFO lot tracking.
6. CAD uses a running-balance method

---

## Beyond Canada — International Use

While AJToken2 is built around Canadian CRA requirements, the core 
methodology applies to any investor who tracks crypto using weighted 
average cost basis (ACB) — a method used in the UK, Australia, and 
many other countries.

The full desktop application includes built-in FIFO lot tracking and 
complete transaction history alongside the ACB calculations — giving 
any crypto investor accurate cost basis tracking and P&L visibility 
regardless of their tax jurisdiction.

The MiniKit companion tools extend this further with standalone 
portfolio pricing and CRA tax packet generation for users who want 
lightweight tools without the full application.

If you hold crypto on Coinbase and want accurate cost basis tracking 
and P&L visibility — wherever you are — AJToken2 provides a solid 
free foundation.

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
## Downloads

| Tool | Download |
|---|---|
| CRA Tax Packet Generator | [ajtoken2-minikit.v0.4.54.exe](https://github.com/Ajumbledbox/Ajtoken2-minikit/releases/tag/Ajtoken2-minikit.v0.4.54) |
| Current Pricing Tool | [ajtoken2.CP.v0.1.11.03.21.2026.1322.hours.N.exe](https://github.com/Ajumbledbox/Ajtoken2-minikit/releases/tag/Ajtoken2-minikit.v0.4.54) |
**© 2013-2026 Ajumbledbox. All rights reserved.**
