# 05 — Grain Analysis

> **Status: COMPLETE — final grain matrix and runtime grain controls validated.**

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
| `dim_product` | one analytical product/member | `product_key` | includes explicit Unmapped member |
| `dim_order_flags` | one unique channel/status/priority combination | `flag_key` | Junk Dimension |
| `dim_geo` | one city/region lookup member | `geo_key` | reused for Ship-To/Bill-To roles |
| `dim_campaign` | one campaign | `campaign_key` | shared by campaign facts |
| `dim_date` | one calendar date | `Date` | shared/role-playing time context |
| `fact_sales` | one order line | `line_id`; `order_id` repeats | additive line Sales/Quantity live at detail grain |
| `fact_inventory` | one product-month snapshot | `product_key` + `month` | monthly Inventory cannot truthfully become daily without allocation |
| `fact_campaign_spend` | one campaign-date activity record | `campaign_key` + `date` | impressions/clicks/spend are dated campaign events |
| `fact_promotion_coverage` | one campaign-product combination | `campaign_key` + `product_key` | factless/coverage-style fact |
| `fact_order_process` | one Order lifecycle row | `order_id` | child events must be reduced to Order milestones |
| `fact_sales_targets` | one target period | `period` | compare to Sales only at a common supported time grain |

## Header/detail control

```text
orders
1 row = 1 Order
        │
        │ OrderID
        ▼
fact_sales
1 row = 1 Order Line
```

`OrderTotal` is an Order-grain value. It must not be repeated and naively summed at Order-Line grain. The final sales fact therefore uses line-level `line_total` as its additive sales value.

## Merge fan-out control

```text
Expected fact_order_process
80 Orders → 80 rows

Naive child-event merge
80 Orders → 97 rows   ❌

Final milestone design
child events aggregated first
→ 80 rows / 80 Orders ✅
```

The final runtime pass confirmed the corrected one-row-per-Order grain.

## Multiple-fact comparison rule

Facts remain separate when they describe different events or grains. They meet through shared dimensions. Comparisons are valid only at a grain both facts understand; periodic Targets are therefore not invented at a finer time grain than the source provides.

## Final evidence

- [x] all dimensions have grain statements
- [x] all six facts have grain statements
- [x] Order vs Order-Line distinction validated
- [x] higher-grain Order values protected from line-level double counting
- [x] order-process fan-out failure documented and corrected
- [x] `fact_order_process` runtime result confirmed at 80 / 80
- [x] Target/common-grain limitation documented
- [x] core measures reconciled after final refresh