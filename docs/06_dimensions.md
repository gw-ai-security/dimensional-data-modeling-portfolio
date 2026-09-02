# 06 — Dimension Design

> **Status: COMPLETE — all guided dimensions implemented and runtime-validated in Power BI.**

## Dimension catalog

| Dimension | Grain | Key | Main source(s) | Purpose |
|---|---|---|---|---|
| `dim_customer` | one customer | `customer_id` | customer master + contacts + user details + address + cities | consolidated customer context and RLS attachment point |
| `dim_product` | one analytical product/member | `product_key` | products + subcategories | product/category context |
| `dim_order_flags` | one channel/status/priority combination | `flag_key` | `orders` + `channels` | Junk Dimension |
| `dim_geo` | one city/region member | `geo_key` | cities | reusable geographic context |
| `dim_campaign` | one campaign | `campaign_key` | campaign log | campaign descriptive context |
| `dim_date` | one calendar date | `Date` | calculated calendar | shared and role-playing time context |

## `dim_customer`

The dimension consolidates fragmented B2B customer context. The source staging query `customer_contacts` is filtered to `IsPrimary = true` before it is merged, protecting the one-customer grain. Final analytical fields include customer identity, segment, account manager, payment terms, primary contact, credit/phone and address/region context.

A redundant Address merge sequence remains in the generated Power Query steps. It does not change the documented output shape but is retained as low-priority technical debt rather than hidden.

## `dim_product`

The dimension flattens product + category/subcategory context, removes technical source fields and creates model key `product_key`.

The source contains guided problem/dummy rows (`ZZZ-000`, `ELE-901`, `HOM-902`) that are handled during transformation.

### Explicit Unmapped member

The first Business Overview exposed sales without a matching Product Category as `(Blank)`. The final design uses:

```text
product_key = 0
product_code = UNMAPPED
product_name = Unmapped Product
category = Unmapped
```

Null product lookups in `fact_sales` are mapped to key `0`. This preserves fact rows and makes the referential-integrity exception visible. The final Power BI refresh confirmed this behavior.

## `dim_order_flags`

Final columns are standardized:

```text
flag_key
channel_code
channel_name
status
priority
```

The `channels` mapping is manually maintained for the controlled course case. In production, an authoritative upstream mapping is preferable because new source codes can otherwise become unmapped.

## `dim_geo`

Built from distinct city/region mappings with model-generated `geo_key`. It is reused by `fact_sales` for geographic roles. Ship-To is the active default relationship; Bill-To is an inactive alternative.

## `dim_campaign`

`CAMPAIGN_LOG` contains descriptive campaign attributes and dated event metrics. Campaign metadata is deduplicated into `dim_campaign`; dated metrics remain in `fact_campaign_spend`.

## `dim_date`

A shared calendar dimension created with `CALENDARAUTO()` is used across Sales, Inventory, Campaign Spend, Sales Targets and the Order Process.

Power BI Auto Date/Time artifacts (`LocalDateTable_*`) remain serialized. The explicit `dim_date` is the intended analytical calendar; the local Auto Date artifacts are documented technical polish, not business dimensions.

## Final validation

- [x] all six dimensions exist in TMDL
- [x] grains and keys documented
- [x] dimension-side behavior validated in the final model
- [x] junk-dimension naming polished
- [x] shared Geo/Date roles validated
- [x] manual mapping caveat documented
- [x] Unmapped Product handling refresh-tested
- [x] final report shows dimension filters behaving as intended