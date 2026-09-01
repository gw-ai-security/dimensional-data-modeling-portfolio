# Lesson 05 — Grain

**Transcript range:** 01:34:29–01:41:27  
**Source:** Data with Baraa — Data Modeling in Power BI Full Course transcript

## 1. Definition

The course defines **grain** as the level of detail represented by one row in a fact table.

The key question is:

> **What does one row represent?**

The course recommends answering that question explicitly in one sentence before doing further modeling or calculations.

Examples:

```text
One row = one order line
One row = one whole order
One row = one month of sales
```

These are three different grains even when the tables refer to the same general business process and may show similar totals.

## 2. Why grain changes the meaning of a fact

Three facts can all describe sales while representing completely different levels of detail.

### Detailed fact — order-line grain

```text
One row = one line of one order
```

An order with three products can create three rows.

### Order-level fact

```text
One row = one complete order
```

The same order is represented once regardless of how many products it contains.

### Monthly fact

```text
One row = one month of sales
```

Individual orders and order lines are no longer visible. The data is already highly aggregated.

This produces a hierarchy of detail:

```text
Order line
    ↓ less detail
Order
    ↓ less detail
Month
```

## 3. Grain is the first question for a fact

The transcript repeatedly emphasizes that understanding grain should happen before:

- deciding how facts connect;
- combining tables;
- writing calculations;
- interpreting totals.

The practical routine is:

```text
See a fact table
→ ask what one row represents
→ state the grain clearly
→ only then model / calculate
```

## 4. Grain controls aggregation logic

A calculation that is correct at one grain can be wrong at another.

### Example: counting orders from an order-line fact

Suppose an order-line fact contains six rows but only three distinct orders.

A simple row count or count of `order_id` would return six and overstate the number of orders.

Because the table grain is **order line**, the correct order count requires counting distinct order IDs.

```text
Wrong assumption:
1 row = 1 order

Actual grain:
1 row = 1 order line

Correct approach:
count distinct order_id
```

The lesson is not specifically about one DAX function. The principle is that aggregation must match the grain.

## 5. A table can contain measures at different grains

A particularly important point in the transcript is that the table grain does not automatically mean every numeric column has the same grain.

Example:

```text
Table grain: one order line
```

Possible columns:

```text
line_sales     → order-line grain
shipping_cost  → whole-order grain repeated on every line
```

If a whole-order shipping cost is repeated across all lines, blindly summing it over the order-line table produces double counting.

This means grain must be understood at two levels:

1. **Table grain** — what one row represents.
2. **Measure/column grain** — at what level a particular number was actually recorded.

## 6. Repeated higher-level values

When an order-level value appears on every line of an order, the repetition is not proof that each line incurred that amount.

The source demonstrates shipping cost as an example of this pattern.

```text
Order 1001
Line 1 → shipping cost 50
Line 2 → shipping cost 50
Line 3 → shipping cost 50
```

The business meaning may still be:

```text
Order 1001 shipping cost = 50
```

not:

```text
50 + 50 + 50 = 150
```

The modeler therefore needs to know whether a value belongs to the row grain or to a higher-level entity.

## 7. Grain and business correctness

The transcript presents grain errors as a direct business risk because they can create apparently valid calculations with incorrect totals.

```text
Misunderstood grain
→ wrong aggregation
→ wrong KPI
→ wrong business decision
```

This is why grain is treated as foundational rather than as an implementation detail.

## 8. Grain statement template

For every fact, document a sentence like:

```text
The grain of fact_sales is one row per order line.
```

For important measures, add:

```text
line_sales is recorded at order-line grain.
shipping_cost is recorded at order grain and is repeated across order lines.
```

This makes later modeling and validation much easier.

## 9. Grain checklist

Before using a fact, ask:

1. What exactly does one row represent?
2. Which columns identify that row-level event?
3. Is the table detailed or already aggregated?
4. Does each numeric column exist at the same grain as the row?
5. Are higher-level values repeated on lower-level rows?
6. Will `SUM`, `COUNT` or another aggregation double count something?
7. Do I need a distinct count or another grain-aware calculation?

## 10. Failure modes from this lesson

- assuming every row is a complete transaction when the grain is a transaction line;
- counting repeated business IDs as separate business events;
- summing values that were recorded at a higher grain and repeated at a lower grain;
- assuming every numeric column has the same grain as the table;
- combining or relating facts before stating their grain;
- trusting matching totals as proof that two facts have the same structure.

## Active Recall

1. What is grain?
2. What is the single most important question to ask when inspecting a fact?
3. What is the difference between order-line grain and order grain?
4. Why can two sales facts with the same total still be structurally different?
5. Why does counting `order_id` in an order-line fact potentially overcount orders?
6. What is the difference between table grain and measure/column grain?
7. Why can summing a repeated shipping-cost column be wrong?
8. What grain statement would you write for a fact where one row represents one product line in an order?

## Learning status

- Theory documentation: ✅
- Lesson watched/studied: ⬜
- Active Recall checkpoint: ⬜
- Capstone implementation evidence: ⬜
