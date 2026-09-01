# 05 — Grain Analysis

> **Status: planned template.** Grain will be documented when the course reaches the grain block and again during the capstone.

## Definition to prove in practice

The grain statement must say exactly what one row represents.

Avoid vague descriptions such as `sales table`.

Prefer statements such as:

> One row represents one product line within one customer order.

## Grain matrix

| Table | Grain statement | Business key(s) | Expected uniqueness | Validation query/check | Result |
|---|---|---|---|---|---|
| | | | | | |

## Why grain matters

Grain determines what can be counted, summed and related safely. A measure such as `number of orders` may require a distinct count if the fact table grain is order line rather than order.

## Validation questions

- Can I state the grain in one precise sentence?
- Which column combination identifies a row at that grain?
- Do the data actually conform to the stated grain?
- Would a merge change the grain?
- Does the intended measure match the fact grain?
