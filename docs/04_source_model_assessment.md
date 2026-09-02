# 04 — Source Model Assessment

> **Status: complete source/business assessment.** The source-shaped Nightmare model has been replaced by the guided dimensional implementation.

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
| `security` | user/region access mapping | dynamic RLS support |
| `exchange_rates` | currency/date lookup source | retained as source/support, not promoted into the final analytical model because no implemented project measure requires it |
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

`fact_order_process` is intended to be one row per order. A naive child-table merge produced a recorded **97 rows for 80 distinct orders**. The final query aggregates shipment/invoice/payment milestones before joining to the order spine, preserving the intended order grain.

## Evidence

- starting model: `screenshots/before/before.png`
- final PBIP/TMDL: `model/nightmare-data-model/`
- grains: `docs/05_grain_analysis.md`
- reconciliation: `tests/reconciliation_tests.md`

## Remaining release distinction

The source assessment itself is complete. Final Power BI Desktop runtime validation of the latest `main` state belongs to `tests/final_validation.md` and Issue #10.