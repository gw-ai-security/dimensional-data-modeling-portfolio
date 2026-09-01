# 04 — Source Model Assessment

> **Status: source investigation completed at coarse business level; guided dimension build has started. Current video checkpoint: 03:45:58.**

## Purpose

Capture the starting business meaning and structural risks before the source-shaped model is replaced by an analytical dimensional model.

## Environment checkpoint

- [x] Power BI Project saved in PBIP/TMDL format
- [x] PBIP project committed under `model/nightmare-data-model/`
- [x] course dataset excluded from Git and documented as local-only source data
- [x] workbook source repointed to repository-local `local-data/dataset.xlsx`
- [x] source model refreshed successfully after repointing
- [x] before-state Model View captured during the workflow
- [ ] before-state image committed under `screenshots/before/`

## Source/business inventory

The first guided investigation intentionally develops a business-level understanding rather than fully reverse-engineering every relationship.

| Source / staging object | Business meaning | Initial grain / shape | Candidate role | Current decision |
|---|---|---|---|---|
| `CUST_MASTER` | customer master/context | one customer record expected | dimension source | consolidated into `dim_customer` |
| `customer_contacts` | customer contacts | contact per customer; primary contact identified | dimension source | primary contact merged into `dim_customer` |
| `user_details` | additional customer/user attributes | one user/customer detail record expected | dimension source | merged into `dim_customer` |
| `Address` | street/city context | one address | dimension source | merged into `dim_customer` |
| `cities` | city-to-region context | one city mapping expected | dimension source | region context merged into `dim_customer` |
| `regions` | region list | one region value | support | not needed as separate customer dimension input once region is available |
| `products` | product master/context | one product | dimension source | transformed into `dim_product` |
| `subcategories` | category/subcategory mapping | one mapping row | dimension source | cleaned/split and merged into `dim_product` |
| `ORDERS_2025` | order headers for 2025 | one row = one order | fact-like staging | append with 2026 |
| `ORDERS_2026` | order headers for 2026 | one row = one order | fact-like staging | append with 2025 |
| `orders` | unified order headers | one row = one order | staging | source for order context/junk dimension and later fact enrichment |
| `order_line_items` | order details | one row = one order line | fact source | planned source for `fact_sales` |
| `INVOICES` | invoice headers | one invoice/event row expected | fact-like source | later fact analysis |
| `invoice_lines` | invoice details | one row = one invoice line | fact-like source | later header/detail analysis |
| `payments` | payment transactions | one payment event | fact-like source | later fact analysis |
| `shipments` / `Sheet1` | shipment events/details | shipment-level rows | fact-like source | later fact analysis |
| `CAMPAIGN_LOG` | marketing campaign activity by date | event/time-series rows | fact candidate | later fact analysis |
| `campaign_skus` | campaign-to-product membership | list/mapping shape | bridge/support candidate | later reshaping |
| `inventory` | stock by product across monthly columns | wide monthly snapshot shape | fact candidate | later unpivot/grain analysis |
| `sales_targets` | target values by period | period-level target | fact candidate | later multiple-fact analysis |
| `exchange_rates` | currency rates by date | currency/date rate | support/fact-like lookup | later analysis |
| `security` | security mapping source | mapping rows | RLS support | later security phase |
| `dim_order` | pre-existing/raw support object | not accepted as final analytical design | source/support | subject to guided remodel |

## Structural findings so far

The imported Nightmare model intentionally starts with source-shaped objects and unreliable auto-detected relationships. The guided approach therefore does not preserve the original graph as the target architecture.

Confirmed modeling observations:

- customer context is fragmented across multiple source tables and is being flattened into one report-friendly `dim_customer`;
- product context is split between product master and category/subcategory mapping and is being consolidated into `dim_product`;
- `ORDERS_2025` and `ORDERS_2026` describe the same business event at the same grain and are consolidated into one `orders` staging query;
- `order_line_items` has a finer grain than the order header and represents the classic transactional **header/detail** pattern;
- order-channel/status/priority attributes are repetitive low-cardinality context and have been extracted into `dim_order_flags`;
- the manually entered channel mapping is convenient for the course case but creates a maintenance dependency if source codes change.

## Current risks / controls

| Risk | Why it matters | Current control |
|---|---|---|
| source auto-relationships | can create wrong cardinality/filter paths | replace deliberately during dimensional build |
| merges that multiply rows | can silently change totals | check uniqueness/cardinality before or after merges |
| header/detail grain mismatch | can create duplicate measures or fact-to-fact design | state grain before combining |
| dummy/test source rows | pollute dimensions and keys | guided filters applied in customer/product dimensions |
| technical/non-analytical columns | increase noise/model size | remove when they do not earn a reporting role |
| manual mapping table | new source codes can become null/unmapped | document ownership; prefer upstream mapping in production |
| no protected fact baseline yet | later transformations could silently alter totals | must record before reshaping first sales fact |

## Before-state evidence

- [x] source/business exploration performed
- [x] initial fact/dimension candidates identified
- [x] initial order/order-line grain distinction confirmed
- [x] major source fragmentation patterns documented
- [ ] before-state screenshot committed to repository
- [ ] protected baseline metric values recorded in `tests/baseline_metrics.md`
- [ ] full final relationship-risk assessment completed after source graph is replaced

## Rule

Do not infer implementation beyond the committed TMDL. At this checkpoint dimensions and staging queries are being built, but the first final fact and final dimensional relationship model do not yet exist.