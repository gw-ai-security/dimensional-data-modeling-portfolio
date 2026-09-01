# Lesson 05 — Grain

**Transcript range:** 01:34:29–01:41:27  
**Source:** Data with Baraa — Data Modeling in Power BI Full Course transcript

## 1. Definition

The course defines **grain** as the level of detail represented by one row in a fact table.

The key question is:

> **What does one row represent?**

Examples:

```text
One row = one order line
One row = one whole order
One row = one month of sales
```

These are different grains even when they describe the same broad business process.

## 2. Detail hierarchy

```text
Order Line  → finer / more detailed
Order       → coarser
Month       → more aggregated
```

An Order can contain multiple Order Lines, so **Order-Line grain is finer than Order grain**.

## 3. Grain must be established first

Before modeling or calculating from a fact:

```text
Inspect fact
→ ask what one row represents
→ state the grain
→ inspect numeric column grain
→ only then model / aggregate / combine
```

Grain must be understood before:

- deciding how facts connect;
- combining tables;
- writing calculations;
- interpreting totals.

## 4. Aggregation depends on grain

If a table is at Order-Line grain, a repeated `order_id` does not represent multiple orders. Counting rows or counting `order_id` without considering grain can therefore overcount business events.

```text
Wrong assumption: 1 row = 1 order
Actual grain:     1 row = 1 order line
```

The calculation must match the business grain.

## 5. Table grain and measure grain can differ

A particularly important point from the transcript is that the table grain does not automatically define the semantic grain of every numeric column.

Example:

```text
Table grain: one order line

line_sales     → order-line grain
shipping_cost  → whole-order grain repeated on each line
```

If the Order-level value is repeated across multiple lines, blindly summing it duplicates the business amount.

This creates two questions:

1. **Table grain** — what does one row represent?
2. **Measure/column grain** — at what business level was this number actually recorded?

## 6. Double-counting example

Recall exercise used during the learning checkpoint:

```text
order_id | product | order_total
1001     | Laptop  | 1,200
1001     | Mouse   | 1,200
1001     | Bag     | 1,200
1002     | Monitor |   500
```

Interpretation:

```text
Table grain             = Order Line
order_total measure grain = Order
```

A naive aggregation gives:

```text
SUM(order_total) = 4,100  ❌
```

because Order 1001 is counted three times.

The correct Order-level total is:

```text
1,200 + 500 = 1,700  ✅
```

The lesson is not to memorize one specific DAX function. It is to prevent aggregation that contradicts the business grain.

## 7. Grain and business correctness

```text
Misunderstood grain
→ wrong aggregation
→ plausible but wrong KPI
→ wrong business conclusion
```

Grain is therefore a semantic modeling requirement, not merely a technical detail.

## 8. Documentation template

For every fact, write an explicit statement:

```text
The grain of fact_sales is one row per order line.
```

For important measures, document exceptions:

```text
line_sales is recorded at order-line grain.
shipping_cost is recorded at order grain and repeated across order lines.
```

## 9. Grain checklist

Before using a fact, ask:

1. What exactly does one row represent?
2. Which columns identify the row-level event?
3. Is the table detailed or already aggregated?
4. Does every numeric column exist at the same grain as the row?
5. Are higher-level values repeated on lower-level rows?
6. Could `SUM`, `COUNT` or another aggregation double count a business event/value?
7. Is a distinct or otherwise grain-aware aggregation required?

## 10. Failure modes

- treating every row as a complete transaction when it is a transaction line;
- counting repeated business IDs as separate events;
- summing values recorded at a higher grain and repeated at a lower grain;
- assuming every numeric column has the same grain as its table;
- combining facts before stating their grains;
- trusting matching grand totals as proof of structural compatibility.

## Active Recall checkpoint — 2026-09-01

**Status: completed after correction and applied example.**

Correctly recalled:

- grain = level of detail / what one row represents;
- table grain and measure grain can differ;
- repeated higher-grain values can cause double counting;
- grain must be understood before combining or comparing facts.

Correction:

- Order grain was initially described as more detailed; corrected to **Order-Line = finer**, **Order = coarser**.

Applied test:

- identified Order-Line table grain;
- identified `order_total` as Order-grain measure;
- diagnosed naive total `4,100` as wrong;
- derived correct Order-level total `1,700`.

## Learning status

- Theory documentation: ✅
- Lesson watched/studied: ✅
- Active Recall checkpoint: ✅
- Capstone implementation evidence: ⬜ not started