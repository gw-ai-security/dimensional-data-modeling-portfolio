# Relationship Validation

> **Status: final TMDL relationship inventory documented; Power BI UI/cardinality/orphan smoke test still pending.**

## Core semantic relationships

| Dimension / context | Fact | Keys | Intended semantic role | Active? | Source review |
|---|---|---|---|---|---|
| `dim_customer` | `fact_sales` | `customer_id` | Customer filters Sales | yes | present |
| `dim_product` | `fact_sales` | `product_key` | Product filters Sales | yes | present |
| `dim_order_flags` | `fact_sales` | `flag_key` | flags filter Sales | yes | present |
| `dim_geo` | `fact_sales` | `geo_key` → `ship_to_city_key` | Ship-To geography default | yes | present |
| `dim_geo` | `fact_sales` | `geo_key` → `bill_to_city_key` | Bill-To alternative role | no | present |
| `dim_product` | `fact_inventory` | `product_key` | Product filters Inventory | yes | present |
| `dim_campaign` | `fact_campaign_spend` | `campaign_key` | Campaign filters Spend | yes | present |
| `dim_campaign` | `fact_promotion_coverage` | `campaign_key` | Campaign filters coverage | yes | present |
| `dim_product` | `fact_promotion_coverage` | `product_key` | Product filters coverage | yes | present |
| `dim_customer` | `fact_order_process` | `customer_id` | Customer filters Order Process | yes | present |
| `dim_date` | `fact_sales` | `Date` ↔ `order_date` | Sales date | yes | present |
| `dim_date` | `fact_inventory` | `Date` ↔ `month` | Inventory month/date | yes | present |
| `dim_date` | `fact_campaign_spend` | `Date` ↔ `date` | Campaign event date | yes | present |
| `dim_date` | `fact_sales_targets` | `Date` ↔ `period` | Target period | yes | present |
| `dim_date` | `fact_order_process` | `Date` ↔ `order_date` | default process date role | yes | present |
| `dim_date` | `fact_order_process` | Ship/Delivery/Invoice/Pay dates | alternative process date roles | no | present |

The TMDL contains no direct fact-to-fact relationship.

## Security relationship/context

A Region relationship between `dim_customer` and `security` exists in TMDL. Dynamic RLS also filters `dim_customer[region]` explicitly through the role expression. Its final cardinality/filter behavior must be verified in Power BI `View As`, not inferred solely from the serialized relationship text.

## Auto Date/Time artifacts

Several `LocalDateTable_*` relationships remain serialized by Power BI. They are not the intended business semantic layer; `dim_date` is. Removing Auto Date/Time artifacts is optional polish and must only be done after confirming no report dependency.

## Release checks

- [x] no direct fact-to-fact relationship in TMDL
- [x] active/inactive role relationships identified
- [x] explicit shared Date relationships identified
- [x] Geo role relationships identified
- [ ] dimension-side key uniqueness rechecked in current Power BI model
- [ ] current `fact_sales.product_key` / `dim_product.product_key` runtime compatibility confirmed after latest Unmapped Product change
- [ ] orphan foreign-key counts rechecked
- [ ] RLS relationship/filter path runtime-tested
- [ ] no ambiguity warning observed after final Refresh