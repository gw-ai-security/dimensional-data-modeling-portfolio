# 08 — Semantic Measures

> **Status: implemented in `_measures`; recorded business values available.**

## Measure catalog

| Measure | Definition | Grain rationale | Recorded value |
|---|---|---|---:|
| `total_sales` | `SUM(fact_sales[line_total])` | `line_total` is additive at Order-Line grain | 526,643.91 |
| `total_orders` | `DISTINCTCOUNT(fact_sales[order_id])` | one Order can have multiple sales lines | 80 |
| `total_active_customers` | `DISTINCTCOUNT(fact_sales[customer_id])` | count customers represented in Sales | 47 |
| `base_total_customers` | `COUNT(dim_customer[customer_id])` | one row per customer in the dimension | 60 |
| `avg_order_to_pay` | `AVERAGE(fact_order_process[order_to_pay])` | process fact is one row per Order | ~32.93 days |
| `total_target_revenue` | `SUM(fact_sales_targets[target_revenue])` | target fact is additive across its recorded periods | 552,000.00 |
| `target_attainment_pct` | `DIVIDE([total_sales], [total_target_revenue])` | compares Sales and Target at compatible report time context | ~95.4% |

Values are recorded from the local audit/course dataset and are not reproducible from GitHub alone because `dataset.xlsx` is intentionally excluded.

## Important semantics

### Total Orders

`COUNTROWS(fact_sales)` would count order lines, not orders. `DISTINCTCOUNT(order_id)` is required because the fact grain is finer than Order grain.

### Average Order → Pay

The measure is only trustworthy if `fact_order_process` remains one row per Order. This is why the fan-out fix is a semantic-measure issue as well as a Power Query issue.

### Target Attainment and RLS

Sales Targets are period-based and not modeled by Region. Regional RLS can restrict Sales while Target remains global. Therefore Target Attainment must not be presented as a region-specific target KPI unless the business supplies region-level targets or the report is explicitly treated as a global/admin view.

## Validation status

- [x] DAX definitions source-controlled
- [x] aggregation choice tied to fact grain
- [x] core recorded values documented
- [x] target/RLS grain caveat documented
- [ ] final post-change Power BI refresh confirms all measures on current `main`
- [ ] Sales Trend and Sales-vs-Target visuals rechecked after Date normalization