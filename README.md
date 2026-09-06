# ASG Airlines — End-to-End Data Engineering Case Study



## Problem Statement
ASG Airlines' flight data (booking platforms, scheduling systems, airport logs) had
corrupted flight identifiers, inconsistent time formats, missing values, and
unhandled overnight (cross-day) flights. This project builds a pipeline that
ingests, cleans, standardizes, and models this data into an analytics-ready
dataset, and reports on it via KPIs and a Power BI dashboard.

## Key Data Quality Issues Found & Fixed
- 41 missing / 6 literal `"UNKNOWN"` airline values — repaired from the flight-number prefix
- 15 duplicate flight rows — dropped
- 1 flight with a corrupted arrival date (arrival before departure) — corrected
- ~12% of flights cross midnight (overnight) — handled via full-datetime duration logic
- Literal `"INVALID"` strings mixed into the numeric `payments.amount` column — coerced and imputed
- 363 bookings with multiple payment rows (retries/instalments) — aggregated to booking grain
- 39 duplicate passenger IDs — deduplicated
- PII (Aadhaar, passport, name, email, phone, DOB) — hashed, masked, or generalized


## Architecture
Local implementation: Python/pandas, run end-to-end in `ASG_Airlines_Pipeline.ipynb`.


## KPIs (from this data extract)
| KPI | Value |
|---|---|
| Average flight duration | 164.6 min (~2h 45m) |
| Overnight flights | 12.1% |
| Cancellation rate | 31.4% |
| Total bookings / revenue | 1,000 / ₹80,05,891.52 |
| Top route by traffic | BOM-CCU (90 flights) |

Full KPI tables: `output/kpi_*.csv`.

## How to Reproduce
1. `pip install pandas numpy openpyxl jupyter`
2. Place the source workbook alongside the notebook and run all cells.
3. Cleaned tables land in `cleaned/`; Power BI-ready tables + KPIs land in `output/`.



