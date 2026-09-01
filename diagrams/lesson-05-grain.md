# Lesson 05 — Grain

Original Mermaid reconstruction of the Grain concepts documented in `docs/theory/lesson_05_grain.md`.

## Grain hierarchy

```mermaid
flowchart LR
    OL["Order Line<br/>1 row = 1 product position"] -->|aggregate| O["Order<br/>1 row = 1 whole order"]
    O -->|aggregate| M["Month<br/>1 row = monthly summary"]
```

Order-Line grain is finer than Order grain.

## Table grain vs measure grain

```mermaid
flowchart TD
    T[Table grain: Order Line] --> L["line_sales<br/>Order-Line grain"]
    T --> OT["order_total<br/>Order grain repeated across lines"]
    OT --> R{Naive SUM across rows?}
    R -->|Yes| X[Double counting / wrong KPI]
    R -->|No: aggregate at business grain| V[Valid result]
```

## Grain-first workflow

```mermaid
flowchart LR
    A[Inspect Fact] --> B[What does exactly one row represent?]
    B --> C[State table grain]
    C --> D[Check measure/column grain]
    D --> E[Choose aggregation / relationship / fact-combination logic]
    E --> F[Validate totals]
```

**Recall:** do not combine or aggregate facts until their grain and the grain of important measures are understood.