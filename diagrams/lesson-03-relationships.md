# Lesson 3 — Relationships

## Healthy star-schema relationship

```mermaid
flowchart LR
    D[Dimension — unique key / 1] -->|single-direction filter| F[Fact — repeating foreign key / many]
```

## Shared dimension instead of direct fact-to-fact

```mermaid
flowchart TD
    P[dim_product — 1] --> S[fact_sales — many]
    P --> B[fact_budget — many]
```

Avoid:

```text
fact_sales  * -------- *  fact_budget
```

## Ambiguous filter paths

```mermaid
flowchart TD
    C[dim_customer] --> S[fact_sales]
    C --> ST[dim_store]
    ST --> S
```

The customer can reach sales through more than one active route. The model must not leave competing active filter paths unresolved.

## Relationship contract

```mermaid
flowchart LR
    R[Relationship] --> K[Key columns]
    R --> C[Cardinality]
    R --> F[Filter direction]
    R --> A[Active / inactive state]
```
