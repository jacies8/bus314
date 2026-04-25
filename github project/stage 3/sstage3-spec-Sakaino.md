# Arcutis Biotherapeutics (ARQT) – Technical Specification

**Created by:** Jacie Sakaino &nbsp;|&nbsp; **Date:** April 2026 &nbsp;|&nbsp; **Version:** 1.0 &nbsp;|&nbsp; **LLM:** Claude Sonnet (Anthropic)
**Role:** Financial Analyst &nbsp;|&nbsp; **Audience:** CFO / Director of FP&A

---

## 1. Problem Statement

Arcutis Biotherapeutics, Inc. (NASDAQ: ARQT) is a publicly traded commercial-stage biopharmaceutical company focused on dermatology therapeutics. This specification outlines the analytical framework for computing 25+ accounting and performance ratios from the company's FY2024 financial statements (with FY2023 as the prior-year baseline), enabling management to assess financial health, operational efficiency, leverage, and value creation during a high-growth commercial ramp.

ARQT's lead asset, ZORYVE (roflumilast), generated $166.5M in net product revenue in FY2024 — a 472% YoY increase — while the company carried an accumulated deficit of $1.25B as of December 31, 2024. All data is sourced from the SEC EDGAR 10-K (filed February 25, 2025) and Q4 2024 Earnings Release. The primary deliverable is a CFO briefing contextualizing each ratio within the commercial-stage biopharma lifecycle.

---

## 2. Inputs (Known Variables)

All figures in USD thousands ($000s). FY2023 prior-year values are used exclusively as opening-balance denominators for return-on-capital and average-basis ratios; FY2024 is the analytical focus.

### Balance Sheet Items

| Variable | Named Range | FY2024 Value | Notes |
|---|---|---|---|
| Cash & cash equivalents | `BAL_cash_2024` | $71,335 | Current asset |
| Marketable securities | `BAL_mkt_securities_2024` | $156,620 | Current asset |
| Trade receivables, net | `BAL_receivables_2024` | $73,066 | Current asset |
| Inventories | `BAL_inventories_2024` | $14,526 | Current asset |
| Total current assets | `BAL_assets_current_2024` | $335,820 | BS subtotal |
| Total assets | `BAL_assets_total_2024` | $348,889 | BS total; prior year: $341,365 |
| Total current liabilities | `BAL_liabilities_current_2024` | $89,123 | BS subtotal |
| Long-term debt, net | `BAL_debt_long_term_2024` | $103,337 | Post $100M repayment |
| Total liabilities | `BAL_liabilities_total_2024` | $196,011 | BS total |
| Shareholders' equity | `BAL_equity_2024` | $30,942 | Prior year: $88,667 |

### Income Statement Items

| Variable | Named Range | FY2024 Value |
|---|---|---|
| Total revenues | `INC_revenue` | $196,510 |
| Cost of sales (COGS) | `INC_cogs` | $19,100 |
| R&D expenses | `INC_rd` | $76,400 |
| SG&A expenses | `INC_sga` | $229,400 |
| EBIT (operating loss) | `INC_ebit` | ($128,390) |
| Interest expense | `INC_int_exp` | $28,680 |
| Net loss | `INC_net` | ($140,336) |
| Depreciation & amortization | `INC_da` | $3,100 |

### Cash Flow & Market Inputs

| Variable | Named Range | FY2024 Value |
|---|---|---|
| Cash from operations | `CASH_operating` | ($90,151) |
| Cash from investing | `CASH_investing` | $24,800 |
| Share price | `mkt_price` | $14.91 |
| Shares outstanding (000s) | `mkt_shares` | 118,638 |
| Market capitalization ($000s) | `mkt_cap` | $1,768,893 |
| Cost of capital (WACC) | `cost_of_capital` | 10.0% |
| Effective tax rate | `tax_rate` | ~0.2% |

---

## 3. Assumptions & Constraints

- All figures in USD thousands ($000s); no unit conversion applied.
- **Tax rate:** Effective rate (~0.2% FY2024) used instead of statutory 21% — NOL carryforwards render the statutory rate inappropriate for a pre-profit company.
- **WACC:** 10.0% analyst estimate; replace with CAPM-derived rate in Stage 4.
- **NOPAT:** `INC_ebit × (1 − tax_rate)` = ($120.0M) FY2024.
- **Start-of-year denominators:** FY2023 Balance Sheet values used as opening balances for all return-on-capital ratios (standard convention).
- **Total Capitalization:** Long-Term Debt + Shareholders' Equity; operating lease liabilities excluded.
- **D&A:** Sourced from management discussion ($3.1M FY2024); not a standalone IS line item.
- **Interest expense:** Stated as absolute value in coverage ratio denominators.
- No off-balance-sheet items, contingent liabilities, or derivative positions included.

---

## 4. Calculation Flow

### Step 1: Derived Inputs
```
mkt_cap               = mkt_price × mkt_shares
INC_nopat             = INC_ebit × (1 − tax_rate)
INC_daily_sales       = INC_revenue / 365
avg_equity            = AVERAGE(startYear_equity, currentYear_equity)
avg_total_assets      = AVERAGE(startYear_assets_total, currentYear_assets_total)
avg_total_cap         = AVERAGE(startYear_total_cap, currentYear_total_cap)
currentYear_nwc       = currentYear_assets_current − currentYear_liabilities_current
currentYear_total_cap = currentYear_ltd + currentYear_equity
```

### Step 2: Performance Ratios
```
MVA             = mkt_cap − currentYear_equity
Market-to-Book  = mkt_cap / currentYear_equity
EVA             = INC_nopat − (cost_of_capital × avg_total_cap)
```

### Step 3: Profitability Ratios
```
ROA (start-of-year)  = INC_net / startYear_assets_total
ROC (start-of-year)  = INC_nopat / startYear_total_cap
ROE (start-of-year)  = INC_net / startYear_equity
ROA / ROC / ROE (average) — repeat with avg_ denominators
```

### Step 4: Efficiency Ratios
```
Asset Turnover       = INC_revenue / avg_total_assets
Recv. Turnover       = INC_revenue / currentYear_receivables
Avg Collection (DSO) = 365 / Receivables Turnover
Inventory Turnover   = INC_cogs / currentYear_inventories
Days in Inventory    = 365 / Inventory Turnover
Net Profit Margin    = INC_net / INC_revenue
Operating Margin     = INC_ebit / INC_revenue
```

### Step 5: Leverage Ratios
```
LT Debt Ratio    = currentYear_ltd / (currentYear_ltd + currentYear_equity)
Debt-to-Equity   = currentYear_ltd / currentYear_equity
Total Debt Ratio = currentYear_liabilities_total / currentYear_assets_total
TIE              = INC_ebit / INC_int_exp
Cash Coverage    = (INC_ebit + INC_da) / INC_int_exp
Debt Burden      = currentYear_ltd / (INC_ebit + INC_da)
Leverage Ratio   = currentYear_assets_total / currentYear_equity
```

### Step 6: Liquidity Ratios
```
NWC-to-Assets  = currentYear_nwc / currentYear_assets_total
Current Ratio  = currentYear_assets_current / currentYear_liabilities_current
Quick Ratio    = (currentYear_assets_current − currentYear_inventories) / currentYear_liabilities_current
Cash Ratio     = currentYear_cash / currentYear_liabilities_current
```

### Step 7: Du Pont Decomposition
```
Du Pont ROE  = (INC_net / INC_revenue) × (INC_revenue / avg_total_assets) × (avg_total_assets / avg_equity)
Du Pont ROA  = (INC_net / INC_revenue) × (INC_revenue / avg_total_assets)
Both should reconcile exactly to ROE (average) and ROA (average) from Step 3.
```

---

## 5. Outputs

| Output | Format | Purpose |
|---|---|---|
| 25+ ratio summary (FY2023 vs. FY2024) | Table | Core CFO deliverable |
| Du Pont decomposition (3-factor ROE, 2-factor ROA) | Table | Identifies return drivers |
| Named-range formula log (Ratios tab, Col. B) | Column | Auditability |
| YoY interpretation notes (Col. F) | Text | Feeds Stage 4 narrative |
| Notes & Assumptions tab | Narrative | Audit trail |

---

## 7. Model Review — What Worked & What to Improve

**What worked well:** Named ranges are applied consistently across all 25+ formulas, making the model traceable and reproducible. The three-factor Du Pont ROE reconciles exactly to the average-basis ROE, confirming arithmetic consistency. Computing both start-of-year and average-basis returns gives the CFO two reference points — useful given ARQT's large mid-year equity raise. The Notes & Assumptions tab documents every input to the SEC filing level.

**What to fix in the improved model:**

- **COGS mapping:** `INC_cogs` was not correctly linked to Cost of Sales, causing Inventory Turnover and Days in Inventory to return zero/undefined. Must be verified in Name Manager before ratio computation.
- **EBIT sourcing:** The `INC_ebit` used in NOPAT (−$93,882) inadvertently nets non-operating income, diverging from IS EBIT (−$128,390). Fix: source `INC_ebit` exclusively from the Operating Loss line.
- **Equity reference:** `currentYear_equity` in the Ratios tab shows $153,064 vs. $30,942 on the Balance Sheet — a cell reference error affecting all equity-based ratios. Must tie to `BAL_equity_2024`.
- **Naming inconsistency:** Some FY2023 inputs mix `startYear_` prefixes with `_FY2023` suffixes. Standardize: raw BS inputs use `_year` suffixes; positional aggregates use `startYear_` / `currentYear_` / `avg_` prefixes only.
- **Color-coding:** Several formula cells share the yellow input color. Enforce: yellow = inputs, green = formulas, blue = assumptions, gray = outputs.

**Additional analysis worth adding:** Industry peer benchmarking (specialty pharma comps), quarterly DSO and cash burn trend, and a breakeven sensitivity table for EBITDA and operating cash flow.

---

## 8. Limitations & Next Steps

This model is a single-company, two-year snapshot with no peer benchmarking, multi-year trend analysis, or forward projections. Off-balance-sheet items, convertible note features, and contingent milestones are excluded. WACC is an analyst estimate, and total revenue includes the $30M Sato License upfront payment, which may overstate sustainable efficiency metrics.

The three errors in Section 7 must be corrected before Stage 4. The corrected named ranges in this spec will feed directly into the Stage 4 AI prompt to interpret ARQT's financial trajectory and draft a CFO briefing memo.

---
