# 08 — Semantic Measures

> **Status: COMPLETE — measures implemented, refreshed and validated in Power BI.**

## Measure catalog

| Measure | Definition | Grain rationale | Final reference |
|---|---|---|---:|
| `total_sales` | `SUM(fact_sales[line_total])` | `line_total` is additive at Order-Line grain | 526,643.91 |
| `total_orders` | `DISTINCTCOUNT(fact_sales[order_id])` | one Order can have multiple sales lines | 80 |
| `total_active_customers` | `DISTINCTCOUNT(fact_sales[customer_id])` | customers represented in Sales | 47 |
| `base_total_customers` | `COUNT(dim_customer[customer_id])` | one row per customer | 60 |
| `avg_order_to_pay` | `AVERAGE(fact_order_process[order_to_pay])` | process fact is one row per Order | ~32.93 days |
| `total_target_revenue` | `SUM(fact_sales_targets[target_revenue])` | target fact is additive across recorded periods | 552,000.00 |
| `target_attainment_pct` | `DIVIDE([total_sales], [total_target_revenue])` | Sales and Target compared at compatible time context | ~95.4% |

The source workbook is intentionally excluded from Git, so the numbers are recorded runtime references rather than values GitHub can recompute independently.

## Important semantics

### Total Orders

`COUNTROWS(fact_sales)` would count order lines, not Orders. `DISTINCTCOUNT(order_id)` is required because Sales grain is finer than Order grain.

### Average Order → Pay

The measure depends on `fact_order_process` preserving one row per Order. The fan-out correction therefore protects both table grain and measure correctness.

### Target Attainment and RLS

Sales Targets are period-based/global rather than Region-based. Regional RLS can restrict Sales while Targets remain global. The report therefore avoids presenting Target Attainment as a region-specific target KPI without corresponding region-level target data.

## Final validation

- [x] DAX definitions source-controlled
- [x] aggregation choice tied to fact grain
- [x] core values reconciled after final refresh
- [x] Sales Trend validated through `dim_date`
- [x] Sales vs Target validated at a compatible common time grain
- [x] target/RLS grain caveat documented and respected