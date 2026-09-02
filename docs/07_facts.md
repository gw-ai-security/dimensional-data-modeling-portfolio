# 07 — Fact Design

> **Status: COMPLETE — all six guided facts implemented and runtime-validated in Power BI.**

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

The higher-grain `OrderTotal` is deliberately removed. `total_sales` is computed from line-level `line_total`, preserving Order-Line grain. `order_date` is normalized to Date for the shared `dim_date` relationship. Unmatched products are preserved through `product_key = 0` rather than deleted.

## `fact_inventory`

The wide source is unpivoted to Product × Month grain:

```text
product_key | month | units
```

The fact connects to `dim_product` and `dim_date`.

## `fact_campaign_spend`

Campaign description is replaced by `campaign_key`; dated metrics remain at campaign-event grain:

```text
campaign_key | date | impressions | clicks | spend
```

## `fact_promotion_coverage`

The comma-separated SKU list is expanded to one row per Campaign × Product combination and model keys are resolved through `dim_campaign` and `dim_product`.

## `fact_order_process` — grain-hardening QA

Declared grain:

> one row = one Order

A naive sequence of one-to-many child merges produced **97 rows for 80 Orders**. The final design aggregates business milestones first:

- shipment: earliest Ship Date + latest Delivery Date per Order;
- payment: latest Pay Date per Invoice;
- invoice/payment: earliest Invoice Date + latest Pay Date per Order;
- reduced milestone sets are then joined to the Order spine.

Runtime validation confirmed the final **80 rows / 80 distinct Orders** grain. `order_to_pay` is therefore safe to average at Order grain.

## `fact_sales_targets`

Period + Target Revenue remain at source planning grain. Sales and Targets are not directly connected; both are filtered through the shared Date dimension and compared only at a compatible common time grain.

## Final validation

- [x] explicit grains documented
- [x] no direct fact-to-fact relationships
- [x] Order-Line vs Order measure grain protected
- [x] order-process fan-out defect diagnosed and fixed
- [x] shared dimension keys used instead of descriptive fact coupling
- [x] final row-count/orphan-key smoke tests completed
- [x] Sales, Targets and Order Process behavior revalidated after final refresh