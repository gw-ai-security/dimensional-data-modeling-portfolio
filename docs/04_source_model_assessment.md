# 04 — Source Model Assessment

> **Status: COMPLETE — source/business assessment and final redesign traceability are closed.**

## Environment evidence

- [x] PBIP/TMDL project stored under `model/nightmare-data-model/`
- [x] `dataset.xlsx` kept local-only and excluded from Git
- [x] source repointed to `local-data/dataset.xlsx`
- [x] Power Query work areas created
- [x] source model inspected before redesign
- [x] before-state image committed as `screenshots/before/before.png`

## Source-to-model decisions

| Source / staging object | Business meaning / grain | Final use |
|---|---|---|
| `CUST_MASTER`, `customer_contacts`, `user_details`, `Address`, `cities` | fragmented customer context | consolidated into `dim_customer` |
| `products`, `subcategories` | product + category context | consolidated into `dim_product` |
| `ORDERS_2025`, `ORDERS_2026` | one row per order | appended into `orders` staging |
| `order_line_items` | one row per order line | foundation of `fact_sales` |
| `shipments`, `INVOICES`, `payments` | lifecycle child events | aggregated into order-level milestones for `fact_order_process` |
| `inventory` | product stock stored in wide monthly columns | unpivoted into `fact_inventory` |
| `CAMPAIGN_LOG` | campaign metadata + dated activity | split into `dim_campaign` + `fact_campaign_spend` |
| `campaign_skus` | campaign-to-product membership | reshaped into `fact_promotion_coverage` |
| `sales_targets` | period target | `fact_sales_targets` |
| `security` | user/region access mapping | Dynamic RLS support |
| `exchange_rates` | currency/date lookup source | retained as support; no implemented measure requires it |
| `dim_order` | contextless source object | not used as a final analytical dimension |

## Structural risks identified

- source auto-relationships were not trusted as target architecture;
- header/detail tables had different grains;
- child-event merges could multiply parent rows;
- technical/hash/source columns added noise and model weight;
- coded order attributes needed analytical context;
- the inventory source required unpivoting before a stable fact grain existed;
- multiple date roles required explicit active/inactive semantics;
- regional access required a deliberate security filter path.

## Important diagnostic result

`fact_order_process` is intended to be one row per Order. A naive child-table merge produced **97 rows for 80 distinct Orders**. The final query aggregates shipment/invoice/payment milestones before joining to the Order spine. Final runtime validation confirmed **80 rows / 80 Orders**.

## Evidence

- starting model: `screenshots/before/before.png`
- final PBIP/TMDL/PBIR: `model/nightmare-data-model/`
- grains: `docs/05_grain_analysis.md`
- reconciliation: `tests/reconciliation_tests.md`
- final closure: `docs/12_final_audit.md`

The source assessment, redesign decisions and final runtime validation now form a complete trace from the chaotic source model to the validated semantic model.