# Relationship Validation

> **Status: COMPLETE — final relationship topology and runtime behavior validated in Power BI.**

## Core semantic relationships

| Dimension / context | Fact | Keys | Semantic role | Active? | Final result |
|---|---|---|---|---|---|
| `dim_customer` | `fact_sales` | `customer_id` | Customer filters Sales | yes | pass |
| `dim_product` | `fact_sales` | `product_key` | Product filters Sales | yes | pass |
| `dim_order_flags` | `fact_sales` | `flag_key` | Order flags filter Sales | yes | pass |
| `dim_geo` | `fact_sales` | `geo_key` → `ship_to_city_key` | Ship-To default geography | yes | pass |
| `dim_geo` | `fact_sales` | `geo_key` → `bill_to_city_key` | Bill-To alternative role | no | pass |
| `dim_product` | `fact_inventory` | `product_key` | Product filters Inventory | yes | pass |
| `dim_campaign` | `fact_campaign_spend` | `campaign_key` | Campaign filters Spend | yes | pass |
| `dim_campaign` | `fact_promotion_coverage` | `campaign_key` | Campaign filters coverage | yes | pass |
| `dim_product` | `fact_promotion_coverage` | `product_key` | Product filters coverage | yes | pass |
| `dim_customer` | `fact_order_process` | `customer_id` | Customer filters Order Process | yes | pass |
| `dim_date` | `fact_sales` | `Date` ↔ `order_date` | Sales date | yes | pass |
| `dim_date` | `fact_inventory` | `Date` ↔ `month` | Inventory month/date | yes | pass |
| `dim_date` | `fact_campaign_spend` | `Date` ↔ `date` | Campaign event date | yes | pass |
| `dim_date` | `fact_sales_targets` | `Date` ↔ `period` | Target period | yes | pass |
| `dim_date` | `fact_order_process` | `Date` ↔ `order_date` | default process date role | yes | pass |
| `dim_date` | `fact_order_process` | Ship/Delivery/Invoice/Pay dates | alternative process date roles | no | pass |

The final TMDL contains no direct fact-to-fact relationship.

## Security relationship/context

Dynamic regional RLS filters `dim_customer[region]` from the `security` mapping. Representative Power BI `View As` scenarios confirmed that the security restriction propagates to customer-related facts as intended.

## Auto Date/Time artifacts

`LocalDateTable_*` artifacts remain serialized by Power BI. They are not the business calendar. `dim_date` is the intended analytical date dimension. Removing Auto Date/Time artifacts is optional technical polish outside the release gate.

## Final checks

- [x] dimension-side key behavior rechecked
- [x] Product key compatibility accepted after final refresh
- [x] explicit Unmapped Product behavior validated
- [x] orphan-key behavior rechecked
- [x] no accidental many-to-many relationship required
- [x] no direct fact-to-fact relationship
- [x] active/inactive Date and Geo roles validated
- [x] RLS filter path runtime-tested
- [x] no unresolved ambiguity warning after final Refresh
- [x] representative visuals cross-filter as intended