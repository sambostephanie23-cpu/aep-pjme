# ⚡ AEP vs. PJME Energy Grid Demand Analysis

## Overview
SQL analysis comparing daily electricity demand between two major U.S. energy grid regions — **AEP (American Electric Power)** and **PJME (PJM Eastern)** — using rolling averages, anomaly detection, and peak demand profiling over a multi-year period.

## Tools & Platform
- **SQL** (BigQuery)
- **Dataset:** AEP & PJME regional energy demand (megawatts)
- **Rows:** ~8,808 | 12 columns

## Key Questions Answered
- How does daily energy demand compare between AEP and PJME regions?
- What are the peak demand hours and values per region?
- Are there anomalous demand spikes, and how frequently do they occur?
- What do rolling 24-hour and 7-day averages reveal about consumption trends?

## SQL Techniques Used
| Technique | Purpose |
|---|---|
| `SUM(CASE WHEN region = ...)` | Pivot-style comparison of AEP vs. PJME in the same row |
| `SAFE_DIVIDE()` | Percentage difference calculation without divide-by-zero errors |
| `AVG() OVER(PARTITION BY region ORDER BY datetime ROWS BETWEEN 24 PRECEDING AND CURRENT ROW)` | Rolling 24-hour average |
| `AVG() OVER(... ROWS BETWEEN 168 PRECEDING AND CURRENT ROW)` | Rolling 7-day average |
| Anomaly flag | Count of readings exceeding threshold |
| `GROUP BY energy_date` | Daily aggregation |

## Output Columns Explained
| Column | Description |
|---|---|
| `pjme_daily` | Total daily MW demand for PJME region |
| `aep_daily` | Total daily MW demand for AEP region |
| `daily_diff` | Absolute demand difference (PJME − AEP) |
| `pct_difference` | Percentage difference between regions |
| `rolling_24h_avg` | 24-hour rolling average per region |
| `rolling_7d_avg` | 7-day rolling average per region |
| `peak_value` | Highest single MW reading in period |
| `anomaly_count` | Number of anomalous readings in period |

## Key Findings
- PJME consistently records higher daily demand than AEP, often with percentage differences exceeding **60%**
- Peak demand hours cluster in the evening across both regions
- Rolling averages smooth out daily noise and reveal longer seasonal demand patterns

## Files
| File | Description |
|---|---|
| `aep_pjme.csv` | Processed daily demand comparison with rolling metrics |
| `screenshots/` | BigQuery SQL query screenshots |

## Data Source
PJM Interconnection & AEP energy demand data — accessed via BigQuery

---
*Project by Stephanie Sambo | Data Analyst | Energy & Utilities | SQL • BigQuery*
