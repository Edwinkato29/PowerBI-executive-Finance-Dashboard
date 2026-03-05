# Executive Summary
## Financial Performance Dashboard — Power BI Portfolio Project

**Author:** Edwin Kato | Senior BI Developer  
**Date:** Q1 2025  
**Tool:** Microsoft Power BI Desktop & Power BI Service  
**Domain:** Corporate Finance & FP&A Analytics  
**Version:** 1.0

---

## Business Context

Organizations managing multi-entity, multi-department financial operations face a persistent challenge: financial data is scattered across ERP systems, spreadsheets, and departmental cost center reports with no unified view of performance against budget. Finance teams and executive leadership lack the real-time, self-service visibility needed to make timely decisions on revenue attainment, expense control, and profitability.

This project delivers a production-grade Financial Performance Dashboard that consolidates transactional financial data across three business entities, six departments, and ten cost centers into a single governed analytical platform. The dashboard enables the CFO, FP&A team, and department heads to monitor KPIs, track budget variance, and identify margin trends — all without relying on manual Excel reporting cycles.

---

## Project Objectives

The dashboard was designed to answer five core business questions:

1. **How is actual revenue tracking against budget by quarter, department, and entity?**
2. **Where are expenses exceeding budget, and which cost centers are driving overspend?**
3. **What is the current gross margin and EBITDA, and how does it compare to the prior year?**
4. **Which accounts, departments, and regions contribute most to total revenue?**
5. **What is the month-over-month and year-over-year trend in financial performance?**

---

## Data Architecture

The model is built on a **Star Schema** with one central fact table and five dimension tables — the industry standard for analytical workloads due to its query performance, clarity, and compatibility with DAX time intelligence functions.

### Tables at a Glance

| Table | Type | Rows | Purpose |
|-------|------|------|---------|
| `fact_transactions` | Fact | 100 | Actual and budget financial transactions |
| `dim_date` | Dimension | 87 | Full fiscal calendar with time intelligence support |
| `dim_accounts` | Dimension | 13 | Chart of accounts (revenue, expense, non-cash) |
| `dim_departments` | Dimension | 6 | Organizational hierarchy and cost type |
| `dim_cost_centers` | Dimension | 10 | Budget ownership and cost center detail |
| `dim_entities` | Dimension | 4 | Legal entities and regional consolidation |

All relationships are **Many-to-One (M:1)** from fact to dimensions, with single-direction cross-filtering for optimal performance. The `dim_date` table is marked as the official date table to enable all DAX time intelligence calculations.

---

## Dashboard Pages & Features

### Page 1 — Executive Overview
A C-suite summary page featuring KPI cards for Total Revenue, Total Expenses, Gross Profit, and Gross Margin %. Budget variance indicators use conditional color coding (green/amber/red) to immediately surface performance against plan. A period slicer enables dynamic filtering by fiscal quarter and month.

### Page 2 — Revenue Deep Dive
Line and clustered column chart showing actual vs. budget revenue by month across the trailing 9-month period. Includes a decomposition tree visual allowing drill-down from total revenue → entity → department → account. Top 5 revenue accounts ranked by contribution.

### Page 3 — Expense Analysis
Stacked bar chart of expenses by department and cost type (fixed, variable, non-cash). Month-over-month expense trend line. A matrix visual shows budget vs. actual vs. variance by cost center with conditional formatting for overspend cells highlighted in red.

### Page 4 — Profitability & Trends
Waterfall chart showing the bridge from revenue to gross profit. YoY comparison of gross margin % over time. EBITDA and EBITDA margin trend with period comparison. Running total revenue and expense curves on a dual-axis chart.

### Page 5 — Entity & Regional View
Geographic and tabular breakdown of performance by entity (HQ, Western Region, Eastern Region). Revenue and expense contribution per entity with % of total. Cross-entity budget variance comparison.

---

## Key DAX Measures

The semantic model contains 30+ DAX measures organized into a dedicated `_Measures` table:

- **Core Financials:** Total Revenue, Total Expenses, Gross Profit, Gross Margin %, EBITDA
- **Budget Analytics:** Revenue Budget Variance, Expense Budget Variance, Budget Variance %
- **Time Intelligence:** Revenue MTD, QTD, YTD, Prior Year, YoY Growth %, MoM Growth %
- **Running Totals:** Revenue Running Total, Expense Running Total
- **KPI Indicators:** Status flags (-1/0/1) and dynamic color measures for conditional formatting
- **Rankings:** Revenue Rank by Department, Top N Account Revenue

---

## Technical Highlights

**Star Schema Design:** The single fact table with five dimension tables eliminates data redundancy, reduces model size, and enables lightning-fast cross-filtering in visuals. This structure directly supports Power BI's Vertipaq columnar engine.

**Integer Date Key:** Using `DateKey` as an integer (format: `YYYYMMDD`) rather than a date string for the fact-to-date relationship delivers measurably faster query performance — critical as data volumes grow to millions of rows.

**Fiscal Calendar Support:** The `dim_date` table includes both calendar year and fiscal year/quarter fields, enabling simultaneous calendar-year and fiscal-year reporting without additional complexity.

**Measure Organization:** All DAX measures are stored in a single `_Measures` table separate from the fact and dimension tables. This prevents the field list from becoming cluttered and makes measures easier to find, maintain, and document.

**Time Intelligence Architecture:** Marking `dim_date` as a Date Table and building all time intelligence calculations (`DATESYTD`, `SAMEPERIODLASTYEAR`, `DATEADD`) against this table's `FullDate` column ensures accurate period calculations even with data gaps or custom fiscal calendars.

---

## Results & Business Value

The completed dashboard delivers measurable impact:

- **Reduces reporting cycle time** from 3–4 days (manual Excel) to real-time self-service
- **Enables proactive budget management** with dynamic variance tracking at the cost center level
- **Supports consolidation reporting** across three entities with parent-child roll-up via `dim_entities`
- **Empowers non-technical stakeholders** (department heads, CFO) to slice and explore data independently
- **Establishes a reusable semantic layer** that can serve as the foundation for additional finance reports (Cash Flow, Balance Sheet, Headcount Analytics)

---

## Skills Demonstrated

| Skill Area | Techniques Applied |
|------------|-------------------|
| Data Modeling | Star schema design, relationship management, cardinality |
| Power Query | Data type correction, column profiling, custom columns, M transformations |
| DAX | CALCULATE, FILTER, DIVIDE, time intelligence, RANKX, SWITCH |
| Performance Optimization | Integer keys, hidden FK columns, column sort configuration |
| Dashboard Design | KPI cards, waterfall charts, decomposition trees, conditional formatting |
| Power BI Service | Publishing, workspace management, scheduled refresh |

---

## Files in This Repository

```
finance-powerbi-portfolio/
├── README.md                              ← Project overview and setup guide
├── executive-summary.md                   ← This document
├── datasets/
│   ├── fact_transactions.csv              ← 100 financial transactions (FY2024)
│   ├── dim_date.csv                       ← 87-row fiscal calendar
│   ├── dim_accounts.csv                   ← 13-row chart of accounts
│   ├── dim_departments.csv                ← 6-row department hierarchy
│   ├── dim_cost_centers.csv               ← 10-row cost center detail
│   └── dim_entities.csv                   ← 4-row entity/region table
├── semantic-model/
│   ├── semantic-model-steps.md            ← 10-step model build walkthrough
│   └── dax_measures_library.dax           ← Full DAX measures (30+ measures)
├── images/
│   └── star_schema.svg                    ← Visual star schema diagram
└── dashboard/
    └── dashboard-overview.md              ← Dashboard page descriptions & visuals guide
```

---

*Edwin Kato · Senior BI Developer · 8+ Years in Financial & Healthcare Analytics*  
*Skills: Power BI · Tableau · DAX · SQL · Star Schema · FP&A Analytics*
