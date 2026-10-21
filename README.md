# Bank Loan Analytics: Credit Risk & Portfolio Performance

![Python](https://img.shields.io/badge/Python-Pandas-blue)
![MySQL](https://img.shields.io/badge/MySQL-KPI%20Analysis-4479A1)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811)
![Tableau](https://img.shields.io/badge/Tableau-Dashboard-E97627)
![Excel](https://img.shields.io/badge/Excel-Power%20Query-217346)

An end-to-end analytics workflow on a **39,717-loan consumer lending portfolio**, from raw, inconsistent source files to validated SQL KPIs and dashboards in Power BI, Tableau, and Excel.

> **Context:** [Describe your own context here, e.g. "Self-directed portfolio project" or your actual internship/course.]
> **Dataset:** Historical consumer loan data (2007–2011).

---

## Business Questions
1. How has lending volume grown over time?
2. How does default risk change across credit grades?
3. Which borrower segments (home ownership, verification status, state) carry the most risk?
4. How much has the portfolio actually returned against what was lent?

## Key Insights
| Area | Finding |
|---|---|
| Volume | **$434.8M** funded across 39,717 loans (2007–2011) |
| Growth | Lending grew from **$2.2M (2007)** to **$256M (2011)** |
| Default rate | **14.59%** on completed loans |
| Risk by grade | Default risk rises from **6% (Grade A)** to **34% (Grade G)** |
| Home ownership | Renters default more (**15.4%**) than mortgage holders (**13.7%**) |
| Revolving balance | Avg **$11K** for Grade A vs **$20K** for Grade G |

## Dashboards
All three dashboards use the same cleaned dataset and the same KPI definitions, so the numbers match across tools.

| Power BI | Tableau | Excel |
|---|---|---|
| ![Power BI](images/SS_PowerBI_dashboard_bank_analytics.png) | ![Tableau](images/SS_Tableu_dashbaord_bank_analytics.png) | ![Excel](images/SS_bank_analytics_Excel_dashboard.png) |

Interactive filters for year, state, grade, and loan status, with KPI cards, trend lines, and segment comparisons.

## Approach

### 1. Data Cleaning & Validation (Python)
The portfolio arrived as two separate files (CSV + Excel) that had to be joined and audited before reporting.
- Verified the join key in both directions, then merged with a one-to-one validated join.
- Found hidden missing values stored as text (`"NA"`, `"N/A"`, `"NONE"`, blank spaces) that standard null checks miss, and converted them with column-specific rules.
- Kept the meaning of missing values as flags (`ever_delinquent`, `has_public_record`, `has_desc`) instead of filling blanks with misleading zeros.
- Converted text fields to proper types: `"36 months"` → `36`, `"11.89%"` → `0.1189`, `"Aug-09"` → date.
- Ran business-rule checks (funded ≤ requested, repayments add up, no payment before issue date) with **0 violations**.
- Investigated anomalies instead of deleting them. For example, 71 loans with no payment date were defaults where all money came from collections.

### 2. Data Modelling & KPI Analysis (MySQL)
- Designed a typed relational table with exact `DECIMAL` types for money and rates, a primary key, and documented nullable columns.
- Wrote KPI queries using window functions (`LAG`) and conditional aggregation:
  - Year-over-year lending growth
  - Revolving balance by grade and sub-grade
  - Payment-to-funded ratio by verification status
  - Loan status by state and credit-pull year
  - Default rate by home ownership (completed loans only)

### 3. Dashboards (Power BI · Tableau · Excel)
Built matching dashboards in three tools to compare how each handles the same KPIs.

## Repository Structure
```
├── data/
│   ├── raw/          # Finance_1.csv, Finance_2.xlsx
│   └── processed/    # loans_clean.csv
├── notebooks/
│   └── 01_cleaning.ipynb
├── sql/
│   ├── 01_setup_and_import.sql
│   └── 02_kpis.sql
├── dashboards/       # .pbix, .twbx, .xlsx
└── images/           # dashboard screenshots
```

## How to Run
1. Run `notebooks/01_cleaning.ipynb` to produce `data/processed/loans_clean.csv`.
2. Run `sql/01_setup_and_import.sql`, then `sql/02_kpis.sql` in MySQL.
3. Open the files in `dashboards/` with Power BI Desktop, Tableau, or Excel.

## Tech Stack
| Layer | Tools |
|---|---|
| Data cleaning | Python, Pandas, NumPy |
| Database & analysis | MySQL (window functions, conditional aggregation) |
| Visualization & BI | Power BI, Tableau, Excel (Power Query) |
