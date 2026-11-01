# Airline Passenger Satisfaction Analysis

End-to-end data analysis of 129,880 airline passenger survey responses: data cleaning, exploratory analysis, SQL validation and an interactive Excel dashboard.

## Project overview

The dataset contains two files (`train.csv` and `test.csv`) with the same 25 columns: passenger profile, trip details, 14 service ratings (1-5), delays and a satisfaction outcome (`Satisfied` or `Neutral or Dissatisfied`).

This project:

1. Cleans and validates the data without losing any rows.
2. Explores what drives satisfaction (segments, services, delays).
3. Delivers the same logic three ways: **Python (Pandas)**, **SQL (PostgreSQL)** and an **Excel dashboard**.

## Key findings

| Area | Result |
|---|---|
| Overall satisfaction | 43.4% of 129,880 passengers |
| Class | Business 69.4%, Eco Plus 24.6%, Eco 18.8% |
| Travel purpose | Business travel 58.4% vs personal travel 10.1% |
| Loyalty | Loyal customers 47.8% vs disloyal 24.0% |
| Distance | About 33% under 1,000 miles, 77% at 3,000+ miles |
| Age | Peaks at 45-54 (57.9%); lowest for under 18 (16.7%) |
| Gender | No meaningful difference (42.9% vs 44.0%) |
| Top service link | Online boarding (72% satisfied when rated 4-5 vs 12% when rated 1-2) |
| Lowest-rated services | Inflight wifi (2.81), ease of online booking (2.88), gate location (2.98) |
| Delays | 47% satisfied on time vs about 36% when 15+ minutes late |

These are associations in survey data, not proof of cause (for example, Business class passengers rate most services higher).

## Repository structure

```
airline-satisfaction-analysis/
├── README.md
├── requirements.txt
├── .gitignore
├── data/
│   ├── train.csv
│   └── test.csv
├── python/
│   └── airline_satisfaction_analysis.py
├── sql/
│   └── airline_satisfaction_analysis.sql
└── excel/
    └── Airline_Satisfaction_Dashboard.xlsx
```

## Excel dashboard

`excel/Airline_Satisfaction_Dashboard.xlsx` contains:

- **Dashboard**: 8 KPI cards, 7 charts, a service scorecard and four dropdown filters (class, travel type, customer type, gender). All numbers are live formulas that recalculate when a filter changes.
- **Summary Report**: executive summary, trends, top/bottom performers, anomalies, recommendations and data-quality notes (full dataset, not affected by filters).
- **Analysis**: the formula tables behind the dashboard.
- **Data Quality**: row reconciliation, check log and cleaning actions.
- **Clean_Data**, **Raw_Train**, **Raw_Test**: processed data and the untouched originals.

## How to run

### Python

```bash
pip install -r requirements.txt
python python/airline_satisfaction_analysis.py data
```

Outputs (cleaned data, quality log, KPI and segment tables) are written to `data/analysis_output/`.

### SQL (PostgreSQL 12+)

1. Open `sql/airline_satisfaction_analysis.sql` and run the staging-table section.
2. Load the CSVs from the `data` folder in `psql`:

```sql
\copy stg_train FROM 'train.csv' WITH (FORMAT csv, HEADER true)
\copy stg_test  FROM 'test.csv'  WITH (FORMAT csv, HEADER true)
```

3. Run the rest of the file. It builds the cleaned view, a PASS/WARN/FAIL validation report, the analysis queries and raw-vs-clean reconciliation checks.

The SQL was also tested on DuckDB and produces the same totals as the Python script and the Excel workbook.

## Cleaning decisions

- `train` and `test` are stacked into one table with a `Source` column; ids do not overlap and there are no duplicate rows.
- A rating of `0` is treated as "not rated" (the scale is 1-5) and excluded from averages. This is an assumption: if 0 means the lowest score, averages for wifi, online booking and schedule convenience would be lower.
- 393 blank `Arrival Delay in Minutes` values are left blank, not imputed.
- Delay outliers (Q3 + 3 x IQR) are flagged but kept.
- Category labels are standardised (for example `disloyal Customer` becomes `Disloyal Customer`).
- No rows are removed: 129,880 raw rows = 129,880 clean rows.

## Limitations

- The data has no dates, routes or airline identifiers, so no trend, route or revenue analysis is possible.
- Satisfaction gaps between services are associations, not causes.
- The Excel dashboard uses dropdown filters and formulas rather than PivotTable slicers.

## Tools

Python (Pandas, NumPy), SQL (PostgreSQL), Microsoft Excel.

## Data source

Add the dataset source and license here (for example the Kaggle page) before publishing.
