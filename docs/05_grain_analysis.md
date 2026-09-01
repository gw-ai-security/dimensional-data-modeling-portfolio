# 05 — Grain Analysis

> **Status: capstone evidence template — theory Grain lesson completed.** Populate this file from the actual Nightmare source model before combining or redesigning fact-like tables.

Theory reference: [`theory/lesson_05_grain.md`](theory/lesson_05_grain.md)

## Purpose

For every fact-like table, state exactly what one row represents and verify that the data conforms to that statement.

Avoid vague descriptions such as:

```text
sales table
```

Prefer explicit statements such as:

```text
One row represents one product line within one customer order.
```

## Grain matrix

| Table | Grain statement | Business key(s) | Important measure grain | Expected uniqueness | Validation check | Result |
|---|---|---|---|---|---|---|
| | | | | | | |

## Measure-grain review

A numeric column does not automatically have the same semantic grain as its table.

For each important measure ask:

- At what business level was this value recorded?
- Is the value repeated across finer-grain rows?
- Would a naive `SUM` or `COUNT` double count it?
- Does the intended report calculation respect the source grain?

Example theory pattern:

```text
Table grain      = Order Line
order_total grain = Order
→ repeated Order total must not be blindly summed across Order Lines
```

## Validation questions

- Can I state the table grain in one precise sentence?
- Which column combination identifies a row at that grain?
- Do the data actually conform to the stated grain?
- Are there duplicates that contradict the grain hypothesis?
- Would an Append preserve the grain?
- Would a Merge multiply rows or change the grain?
- Does each important measure match the table grain or a higher grain?
- What is the lowest valid comparison grain when this fact is compared with another fact?

## Evidence required during the capstone

- [ ] grain statement for every fact-like table
- [ ] business key / row-identity hypothesis
- [ ] uniqueness or duplicate check
- [ ] important measure-grain exceptions documented
- [ ] risks of double counting identified
- [ ] Append/Merge/separate-fact decision justified where relevant
- [ ] final grain statements reconciled with the implemented model