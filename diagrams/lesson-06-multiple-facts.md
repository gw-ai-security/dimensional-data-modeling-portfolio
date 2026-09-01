# Lesson 06 — Multiple Facts

Original Mermaid reconstruction of the multiple-fact decision logic documented in `docs/theory/lesson_06_multiple_facts.md`.

## Append / Merge / Separate decision

```mermaid
flowchart TD
    A[Two Fact-like tables] --> B{Same business event?}
    B -->|Yes| C{Same grain and compatible shape?}
    C -->|Yes, same structure| D[APPEND rows]
    C -->|Same grain, 1:1 complementary data| E[MERGE when justified]
    B -->|No / different event| F[KEEP SEPARATE]
    C -->|Different grain| F
    F --> G[Connect through Shared Dimensions]
    G --> H[Compare only at a grain both Facts understand]
```

## Shared dimension pattern

```mermaid
flowchart LR
    P[dim_product] -->|1:*| S[fact_sales]
    P -->|1:*| B[fact_budget]
    D[dim_date] -->|1:*| S
    D -->|appropriate shared grain| B
```

## Common-grain comparison

```mermaid
flowchart LR
    DS[Daily Sales] -->|aggregate| MS[Monthly Sales]
    MB[Monthly Budget] --> C[Compare at Month]
    MS --> C
```

**Do not:** directly connect two Facts through repeating keys, merge different-grain Facts and create fan-out, or invent detail below the grain where a measure was recorded.