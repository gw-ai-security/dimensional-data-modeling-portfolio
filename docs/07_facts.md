# 07 — Fact Design

> **Status: not yet implemented at video checkpoint 03:45:58. First fact is the next guided build step.**

This file distinguishes source fact candidates from facts that actually exist in the analytical model.

## Current fact-design boundary

The committed model currently contains staging queries and implemented dimensions, but no completed analytical `fact_sales` table yet.

The next guided sequence begins after the final `dim_order_flags` polish and uses `order_line_items` as a reference/source for the first fact.

## Planned first fact — `fact_sales`

### Business process

Sales/order-line activity.

### Intended grain

> One row represents one product line/position within an order.

This is derived from the confirmed `order_line_items` source grain. `OrderID` can repeat because one order can contain several line items.

### Source measures already visible in `order_line_items`

| Source column | Meaning | Grain/caveat |
|---|---|---|
| `Quantity` | quantity on the line | order-line grain |
| `UnitPrice` | unit selling price | line/product context |
| `UnitCost` | unit cost | line/product context |
| `DiscountPct` | discount percentage | line-level attribute/measure context |
| `LineTotal` | total for the order line | order-line grain |

### Header/detail decision

The transactional source stores one business event across two levels:

```text
orders
1 row = 1 order header
        │
        │ OrderID
        ▼
order_line_items
1 row = 1 order line/detail
```

The analytical fact should not be created as two directly connected facts merely because header and detail both look transactional. The measurements are at the detail level, so the guided project uses the detail as the fact foundation and later brings required header context to it under controlled merge/lookup steps.

### Required controls before/while building

- [ ] create `fact_sales` as a reference of `order_line_items`
- [ ] record the protected baseline sales total before risky merges
- [ ] verify the order-line grain remains unchanged
- [ ] merge order-header context without multiplying rows
- [ ] re-check the protected total after the merge
- [ ] add dimension IDs/keys through controlled lookups
- [ ] remove descriptive attributes once their authoritative dimension/key exists
- [ ] connect dimensions only after the fact shape is stable

## Other source fact candidates — later phases

| Source | Business event / shape | Current status |
|---|---|---|
| `INVOICES` + `invoice_lines` | invoice header/detail | pending guided analysis/build |
| `payments` | payment transaction | pending |
| `shipments` / `Sheet1` | shipment process | pending |
| `CAMPAIGN_LOG` | campaign activity over time | pending |
| `inventory` | monthly stock/snapshot in wide source format | pending |
| `sales_targets` | period-level targets | pending |

## Claim boundary

The table name `fact_sales` and its intended grain are documented here because they are the immediate next guided step and are supported by the source/grain analysis. They are **not** marked implemented until the corresponding TMDL table exists in the repository.