# Lesson 1 — Modeling Foundations

## Position of the data model

```mermaid
flowchart LR
    A[Sources] --> B[Power Query / transformation]
    B --> C[Data Model]
    C --> D[DAX calculations]
    D --> E[Reports / visuals]
```

## Why the one-big-flat-table approach becomes problematic

```mermaid
flowchart TD
    A[One big flat table] --> B[Repeated descriptive data]
    B --> C[Larger model]
    C --> D[Slower refresh]
    C --> E[Slower report interaction]
    A --> F[Model structure must be compensated in calculations]
    F --> G[More complicated DAX]
```

## One table → one report problem

```mermaid
flowchart TD
    A[Business rule] --> B[Report-specific model A]
    A --> C[Report-specific model B]
    A --> D[Report-specific model C]
    B --> E[Duplicated logic]
    C --> E
    D --> E
    E --> F[Higher maintenance effort]
    F --> G[Inconsistent KPI definitions]
    G --> H[Loss of trust]
```

## Core data model components

```mermaid
flowchart LR
    M[Data Model] --> T[Tables — store]
    M --> R[Relationships — connect]
    M --> C[Calculations — answer business questions]
```

## Fact and dimension roles

```mermaid
flowchart LR
    C[dim_customer] --> F[fact_sales]
    P[dim_product] --> F
    D[dim_date] --> F

    F --> X[Events / activities / numeric values]
    C --> Y[Descriptive context]
    P --> Y
    D --> Y
```
