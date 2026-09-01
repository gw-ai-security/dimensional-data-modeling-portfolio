# 05 — Grain Analysis

> **Status: active capstone evidence. Current guided checkpoint: 03:45:58.**

Theory reference: [`theory/lesson_05_grain.md`](theory/lesson_05_grain.md)

## Purpose

For every analytical object, state what exactly one row represents before merging, appending, aggregating or connecting tables.

## Confirmed / working grain matrix

| Table | Grain statement | Key / row identity | Modeling consequence | Status |
|---|---|---|---|---|
| `ORDERS_2025` | one row represents one order header | `OrderID` | same grain/event as 2026; eligible for Append | confirmed in guided analysis |
| `ORDERS_2026` | one row represents one order header | `OrderID` | same grain/event as 2025; eligible for Append | confirmed in guided analysis |
| `orders` | one row represents one order header after year consolidation | `OrderID` | unified staging object for order-level context | implemented |
| `order_line_items` | one row represents one line/position within an order | `LineID`; `OrderID` repeats | finer grain than order header; planned foundation for sales fact | confirmed |
| `dim_customer` | one row represents one customer | `customer_id` expected unique | dimension-side uniqueness must be protected during merges | implemented |
| `dim_product` | one row represents one product | model `product_key`; source `product_code` business key | model surrogate key introduced | implemented |
| `dim_order_flags` | one row represents one distinct combination of order channel, status and priority | `flag_key` | junk-dimension grain is the distinct attribute combination | implemented |
| `INVOICES` | one row is expected to represent one invoice header | `InvoiceID` | later header/detail fact decision | pending deeper validation |
| `invoice_lines` | one row represents one invoice line | `InvoiceLineID`; `InvoiceID` repeats | finer grain than invoice header | pending later fact build |
| `payments` | one row represents one payment transaction | `PaymentID` | independent business event/fact candidate | pending later fact build |
| `shipments` | one row represents one shipment event | `ShipmentID` | independent business event/fact candidate | pending later fact build |

## Header/detail pattern

The current project has reached the classic transactional pattern:

```text
ORDER HEADER
orders
1 row = 1 order
        │
        │ OrderID
        ▼
ORDER DETAILS
order_line_items
1 row = 1 order line
```

The detail table has the finer grain. One `OrderID` can therefore occur on multiple order-line rows. This is expected behavior, not a duplicate-row defect.

The analytical fact should be modeled at the level of what is being measured. In the guided project the next planned `fact_sales` starts from `order_line_items`, so its intended row grain is the order-line grain. This is a **planned next step**, not yet an implementation claim at 03:45:58.

## Append decision already applied

`ORDERS_2025` and `ORDERS_2026` are split by the source system but represent the same event at the same grain. They are therefore appended into `orders`:

```text
ORDERS_2025 ─┐
             ├─ APPEND → orders
ORDERS_2026 ─┘

Grain before: one order
Grain after:  one order
```

The Append preserves the row meaning while removing an unnecessary source-system split from the analytical preparation layer.

## Dimension grains now visible

```text
dim_customer
1 row = 1 customer

dim_product
1 row = 1 product

dim_order_flags
1 row = 1 unique combination of:
channel + status + priority
```

The junk-dimension example is important because its row identity is not a source business entity ID. Its grain is the **unique combination of descriptive flags** and the model-generated `flag_key` identifies that combination.

## Measure-grain controls for the next phase

Before `fact_sales` is reshaped, record the protected total requested by the guided project. After each merge that can change row multiplicity, re-check the same total.

Questions that remain mandatory:

- does a merge preserve the order-line row count/grain?
- does a lookup dimension contain one matching row per lookup value?
- are any order-header measures repeated after bringing header context to order lines?
- does a numeric column belong to Order grain or Order-Line grain?
- can a naive `SUM` double count a higher-grain amount?

## Evidence status

- [x] Order header grain documented
- [x] Order-line grain documented
- [x] year-split Append justified by same grain/event
- [x] current dimension grains documented
- [x] header/detail risk explicitly documented
- [ ] protected sales baseline recorded
- [ ] first `fact_sales` grain validated after implementation
- [ ] remaining fact grains validated
- [ ] final grain statements reconciled with final model