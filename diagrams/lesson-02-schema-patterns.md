# Lesson 2 — Schema Patterns

## Star schema

```mermaid
flowchart LR
    C[dim_customer] --> F[fact_sales]
    P[dim_product] --> F
    D[dim_date] --> F
    S[dim_store] --> F
```

**Course default:** facts in the center, dimensions around them.

## Filter propagation concept

```mermaid
flowchart LR
    P[dim_product: Category = Electronics] -->|filter| F[fact_sales]
    F --> M[Sales measure in filtered context]
```

## Snowflake schema

```mermaid
flowchart LR
    C[dim_category] --> P[dim_product]
    P --> F[fact_sales]
    D[dim_date] --> F
    U[dim_customer] --> F
```

A dimension is split into additional dimension-level tables. This can reduce duplication/model size in some cases but introduces more relationships and longer filter paths.

## Galaxy schema

```mermaid
flowchart LR
    P[dim_product] --> S[fact_sales]
    P --> B[fact_budget]
    D[dim_date] --> S
    D --> B
```

Multiple facts share common dimensions. Facts are not connected directly.
