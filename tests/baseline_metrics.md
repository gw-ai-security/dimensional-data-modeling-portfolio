# Baseline Metrics

> **Status: populated from the local 2026-09-02 model/source audit.**

The source workbook is intentionally local-only. These values are recorded validation references, not hard-coded targets that the model should be manipulated to match.

| Metric | Business definition | Grain / aggregation | Recorded value | Evidence / method | Status |
|---|---|---|---:|---|---|
| Order Lines | detail rows in sales source/fact | count Order-Line rows | 200 | local dataset/model audit | recorded |
| Total Orders | unique sales Orders | `DISTINCTCOUNT(order_id)` | 80 | local model + Business Overview | recorded |
| Total Sales | sum of line sales | `SUM(line_total)` at Order-Line grain | 526,643.91 | local model + Business Overview | recorded |
| Active Customers | customers with Sales | `DISTINCTCOUNT(customer_id)` | 47 | Business Overview | recorded |
| Base Customers | rows in `dim_customer` | one row per customer | 60 | local model audit | recorded |
| Total Target Revenue | sum of period targets | target fact period grain | 552,000.00 | local audit | recorded |
| Target Attainment | Sales / Target | common report time context | ~95.4% | Business Overview | recorded |
| Order Process rows | one process row per Order | row count | 80 intended | hardened design/local audit | final runtime recheck pending |
| Distinct Process Orders | unique `order_id` | distinct count | 80 | local audit | final runtime recheck pending |
| Orders with Payment | Orders with non-null Pay Date | Order grain | 60 | local audit | recorded |
| Average Order → Pay | average day difference | Order grain | ~32.93 days | local audit | recorded |

## Diagnostic control

A naive Order Process merge produced **97 rows for 80 Orders**. That number is not a valid business baseline; it is a recorded failure signal used to prove the final merge design needed to change.

## Release rule

If a final Power BI refresh returns a different core metric, investigate the transformation/relationship path. Do not alter data merely to force these reference values.