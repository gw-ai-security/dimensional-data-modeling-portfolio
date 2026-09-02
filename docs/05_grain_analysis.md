# 05 — Grain Analysis

> **Status: final implementation grain matrix documented.**

Theory reference: [`theory/lesson_05_grain.md`](theory/lesson_05_grain.md)

## Grain rule

For every analytical table ask:

> **What does exactly one row represent?**

Grain is treated as a contract. A successful merge that changes row meaning is a modeling defect.

## Final grain matrix

| Object | Grain | Primary identity / important key | Consequence |
|---|---|---|---|
| `orders` | one order header | `OrderID` | staging context; do not sum Order-level values after expansion to lines |
| `dim_customer` | one customer | `customer_id` | dimension lookup |
| `dim_product` | one analytical product/member | `product_key` | includes explicit unmapped member in latest source design |
| `dim_order_flags` | one unique channel/status/priority combination | `flag_key` | junk dimension |
| `dim_geo` | one city/region lookup member | `geo_key` | reused for Ship-To/Bill-To roles |
| `dim_campaign` | one campaign | `campaign_key` | shared by campaign facts |
| `dim_date` | one calendar date | `Date` | shared/role-playing date context |
| `fact_sales` | one order line | `line_id`; `order_id` repeats | additive line sales/quantity live at detail grain |
| `fact_inventory` | one product-month snapshot | `product_key` + `month` | monthly inventory cannot truthfully become daily without an allocation/assumption |
| `fact_campaign_spend` | one campaign-date activity record | `campaign_key` + `date` | impressions/clicks/spend are dated campaign events |
| `fact_promotion_coverage` | one campaign-product combination | `campaign_key` + `product_key` | factless/coverage-style fact |
| `fact_order_process` | one order lifecycle row | `order_id` | accumulating process snapshot; child events must be reduced to order milestones |
| `fact_sales_targets` | one target period | `period` | compare to sales only at a common supported time grain |

## Header/detail control

```text
orders
1 row = 1 order
        │
        │ OrderID
        ▼
fact_sales
1 row = 1 order line
```

`OrderTotal` is an Order-grain value. It must not be repeated and naively summed at Order-Line grain. The final sales fact therefore uses line-level `line_total` as its additive sales value.

## Merge fan-out control

The local QA exposed why grain must be verified after Merges:

```text
Expected fact_order_process
80 orders → 80 rows

Naive child-event merge
80 orders → 97 rows   ❌

Final milestone design
child events aggregated first
→ one process row per order
```

## Multiple-fact comparison rule

Facts remain separate when they describe different events or grains. They meet through shared dimensions. Comparisons are only valid at a grain both facts understand; for example, periodic targets must not be invented at a finer time grain than the source provides.

## Evidence status

- [x] all final dimensions have grain statements
- [x] all six final facts have grain statements
- [x] Order vs Order-Line distinction documented
- [x] order-process fan-out failure documented
- [x] target/common-grain limitation documented
- [ ] final runtime row-count proof for current `main` state captured in Power BI evidence