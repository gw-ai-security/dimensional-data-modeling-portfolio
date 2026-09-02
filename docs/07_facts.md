# 07 — Fact Design

> **Status: all six guided facts implemented in PBIP/TMDL.**

## Fact catalog

| Fact | Grain | Core values / purpose | Main dimensions |
|---|---|---|---|
| `fact_sales` | one order line | quantity, price, cost, discount, line total | Customer, Product, Order Flags, Geo, Date |
| `fact_inventory` | one product-month | units | Product, Date |
| `fact_campaign_spend` | one campaign-date record | impressions, clicks, spend | Campaign, Date |
| `fact_promotion_coverage` | one campaign-product combination | coverage/membership | Campaign, Product |
| `fact_order_process` | one order | lifecycle dates, `order_to_pay` | Customer, Date roles |
| `fact_sales_targets` | one target period | target revenue | Date |

## `fact_sales`

Built from `order_line_items` and enriched through controlled lookups to Order, Customer, Product, Order Flags and Geo context. Descriptive context is removed after model keys are assigned.

The higher-grain `OrderTotal` is deliberately removed. `total_sales` is computed from line-level `line_total`, preserving the Order-Line grain.

`order_date` is normalized to Date so the shared `dim_date` relationship can match cleanly.

The latest source design maps unmatched products to `product_key = 0` rather than deleting fact rows.

## `fact_inventory`

The source stores months as columns. Power Query unpivots the wide structure into:

```text
product_key | month | units
```

The fact then connects to `dim_product` and the shared `dim_date`.

## `fact_campaign_spend`

Campaign description is removed in favor of `campaign_key`; dated metrics remain additive event values:

```text
campaign_key | date | impressions | clicks | spend
```

## `fact_promotion_coverage`

The comma-separated campaign SKU list is expanded to one row per Campaign × Product combination and model keys are resolved through `dim_campaign` and `dim_product`.

## `fact_order_process` — grain-hardening QA

Intended grain:

> one row = one order

The local audit found that direct one-to-many merges could produce 97 rows for 80 orders. The final query therefore builds milestones first:

- shipment: earliest Ship Date + latest Delivery Date per Order;
- payment: latest Pay Date per Invoice;
- invoice/payment: earliest Invoice Date + latest Pay Date per Order;
- merge the reduced milestone sets to the Order spine.

`order_to_pay` is then calculated from Order Date to Pay Date.

This prevents child-event fan-out from corrupting order-level averages.

## `fact_sales_targets`

Keeps Period + Target Revenue at source planning grain. It is not directly related to Sales; both facts are filtered through the shared Date dimension and should only be compared at a time grain supported by both.

## Fact validation status

- [x] explicit fact grains documented
- [x] no direct fact-to-fact relationships in TMDL
- [x] Order-Line vs Order measure grain protected
- [x] order-process fan-out defect diagnosed and query hardened
- [x] shared dimension keys used instead of descriptive fact context
- [ ] final Power BI runtime row-count/orphan-key smoke test for latest `main` state