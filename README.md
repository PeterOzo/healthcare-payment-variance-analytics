# Healthcare Payment Variance Analytics and Contract Modeling Simulator
Production-grade CMS Medicare payment variance analytics and contract modeling simulator. Identifies $16.66B in underpayment across 145,879 hospital-DRG combinations using FY2025 IPPS formula.

## Overview
A production-grade payment variance analytics system built on real CMS Medicare FY2022 
inpatient data. Implements the FY2025 IPPS expected payment formula to identify and 
quantify underpayment across 145,879 hospital-DRG combinations covering 4.95 million 
Medicare discharges and 2,906 hospitals nationwide.

## Key Findings
- **$16.66 billion** in total underpayment exposure identified
- **88.5%** of hospital-DRG pairs exhibit statistically significant underpayment  
- National average yield ratio: **0.818** (81.8 cents per dollar of expected payment)
- Best contract renegotiation scenario: Stop-loss threshold reduction → **$184.2B uplift**

## Technical Architecture
- **ETL:** Python (pandas, pyarrow) with 4-encoding fallback cascade for CMS files
- **Validation:** Pandera schema enforcement at every data layer boundary
- **Analytics:** DuckDB in-memory SQL — window functions, CTEs, CMI computation
- **Formula:** FY2025 IPPS (IFC-corrected base rate $6,624.39, additive IME+DSH)
- **Export:** Parquet (snappy) + Excel workbooks → Power BI ready

## Data Sources (all public, no DUA required)
| Dataset | Source | Key Stats |
|---|---|---|
| CMS Medicare Inpatient FY2022 | data.cms.gov | 145,879 rows, 2,906 CCNs, 540 DRGs |
| IPPS Table 5 FY2025 (CN file) | cms.gov | 771 MS-DRGs, weights 0.547–37.71 |
| IPPS Impact File FY2025 (IFC) | cms.gov | 3,159 hospitals, wage index/IME/DSH |
| CMS Outpatient Provider CY2022 | data.cms.gov | APC-level payment data |
| CY2025 Physician Fee Schedule | cms.gov | 10,000+ HCPCS, CF=$32.35/RVU |

## Critical Implementation Details
1. **IFC-corrected base rate:** $6,624.39 (not $6,394.36 — $1.14B difference in aggregate)
2. **Additive IME+DSH:** Applied as (1 + IME + DSH), not multiplicatively
3. **Zero-padded string keys:** CCN (6-char) and DRG (3-char) — never cast to integer

## Contract Scenario Modeling
| Scenario | Parameter | Revenue Uplift |
|---|---|---|
| S1 Rate Escalation | 190% → 215% of Medicare | $174.3B |
| S2 Stop-Loss Reduction ⭐ | $100K → $75K threshold | **$184.2B** |
| S3 Denial Rate Halved | 8% → 4% (operational) | $163.8B |
| S4 DRG Carve-Out | Weight >3.0 at 125% Medicare | $137.3B |

## Deliverables
- `Project1_Payment_Variance_Analytics.ipynb` — 52-cell Colab notebook
- `fct_payment_variance.parquet` — 145,879-row variance fact table
- `dim_state_underpayment.parquet` — 51 states ranked by exposure
- `mart_scenario_results.parquet` — All 4 contract scenarios
- `payment_variance_analytics.xlsx` — 6-sheet Power BI-ready workbook
- 6 interactive Plotly HTML charts

## Technologies
Python · pandas · NumPy · DuckDB · Pandera · Plotly · Parquet · Google Colab

## Author
**Peter Chika Ozo-ogueji**  
M.S. Data Science (GPA 3.83) · M.S. Business Analytics & AI (GPA 3.52)  
American University · po3783a@american.edu  
ORCID: 0009-0003-7898-4644


