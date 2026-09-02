# Baseline Metrics

> **Status: FINAL — references reconciled against the completed Power BI model on 2026-09-02.**

The source workbook is intentionally local-only. These values are validation references, not hard-coded targets that the model should be manipulated to match.

| Metric | Business definition | Grain / aggregation | Final value | Evidence / method | Status |
|---|---|---|---:|---|---|
| Order Lines | detail rows in Sales | count Order-Line rows | 200 | local dataset/model audit | pass |
| Total Orders | unique Sales Orders | `DISTINCTCOUNT(order_id)` | 80 | model + Business Overview | pass |
| Total Sales | sum of line sales | `SUM(line_total)` at Order-Line grain | 526,643.91 | model + Business Overview | pass |
| Active Customers | customers with Sales | `DISTINCTCOUNT(customer_id)` | 47 | Business Overview | pass |
| Base Customers | rows in `dim_customer` | one row per customer | 60 | model audit | pass |
| Total Target Revenue | sum of period targets | target fact period grain | 552,000.00 | model/report audit | pass |
| Target Attainment | Sales / Target | compatible report time context | ~95.4% | Business Overview | pass |
| Order Process rows | one process row per Order | row count | 80 | final runtime validation | pass |
| Distinct Process Orders | unique `order_id` | distinct count | 80 | final runtime validation | pass |
| Orders with Payment | Orders with non-null Pay Date | Order grain | 60 | model audit | pass |
| Average Order → Pay | average day difference | Order grain | ~32.93 days | model audit | pass |

## Diagnostic control

A naive Order Process merge produced **97 rows for 80 Orders**. That number is intentionally retained as failure evidence. It proved that technically successful one-to-many child merges violated the declared Order grain.

## Final rule

Core measures and row identities reconcile after the final refresh. Any future change that alters these values must be investigated at the transformation, grain or relationship layer rather than hidden with report filters.