# 02 — Star, Snowflake and Galaxy Schemas

> Source basis: Data with Baraa Data Modeling full-course transcript. Terminology and decision framing follow the course.

## 1. Star schema

The star schema places the fact table in the center and directly connects dimensions around it.

```text
             dim_date
                 |
dim_customer — fact_sales — dim_product
                 |
             dim_store
```

The fact represents the event/numbers being analyzed. Dimensions provide the context used to filter and group those facts.

### Why the course favors it

The star schema is presented as the default analytical model because it provides:

- a clear structure
- direct dimension-to-fact relationships
- predictable filter propagation
- simpler calculation logic than a poorly structured model

A useful reporting heuristic is:

- looking for a value/measure → think **fact**
- looking for a slicer, label or grouping → think **dimension**

## 2. Filter propagation

The conceptual direction established in the course is:

```text
Dimension → Fact → Measure result
```

For example, selecting one product/category in a dimension filters the related fact rows, and a sales measure is then evaluated in that filtered context.

## 3. Snowflake schema

A snowflake schema appears when a dimension is further split into related dimension tables.

Example:

```text
dim_category → dim_product → fact_sales
```

The course describes the trade-off as follows:

### Potential benefit

- a very large dimension can be split, potentially reducing repetition/model size

### Costs

- more relationships
- longer filter paths
- increased model complexity
- potentially more work for the engine and calculations

### Course default

Use a **star schema by default**. Snowflaking is a deliberate exception when the dimension structure/size justifies the additional complexity.

## 4. Galaxy schema

A galaxy schema contains multiple fact tables that share dimensions.

Example:

```text
             dim_product
              /       
      fact_sales     fact_budget
              \       /
               dim_date
```

The shared dimensions provide common analytical context across the facts.

## 5. Critical rule: do not connect facts directly

Avoid a direct many-to-many fact relationship such as:

```text
fact_sales * -------- * fact_budget
```

Instead, connect both facts through a shared dimension with a unique key:

```text
fact_sales * — 1 dim_product 1 — * fact_budget
```

## 6. Decision summary

| Pattern | Structure | Course use |
|---|---|---|
| Star | one fact with direct dimensions | default |
| Snowflake | dimension split into additional dimension tables | exception when justified |
| Galaxy | multiple facts sharing dimensions | multi-fact analytical model |

## Current evidence status

- Concept documented: ✅
- Independent recall performed: ✅
- Capstone implementation: ⬜ not started
