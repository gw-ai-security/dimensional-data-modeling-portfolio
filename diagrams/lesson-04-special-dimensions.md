# Lesson 04 — Special Dimensions

Original Mermaid reconstruction of the concepts documented in `docs/theory/lesson_04_special_dimensions.md`.

## Extract dimensions hidden in facts

```mermaid
flowchart LR
    F[Fact with repeated descriptive attributes] --> Q{Do attributes describe one coherent business concept?}
    Q -->|Yes| D[Extract normal dimension]
    Q -->|No, small heterogeneous flags| J[Consider Junk Dimension]
    D --> K[Create unique dimension key]
    J --> K2[Create unique combination key]
    K --> M[Merge key back into Fact]
    K2 --> M
    M --> C[Remove repeated descriptive columns from Fact]
```

## Role-playing dimension

```mermaid
flowchart LR
    E[dim_employee] -->|Active: salesperson role| F[fact_sales]
    E -.->|Inactive: manager role| F
    U[USERELATIONSHIP in a specific measure] -.->|Uses alternative role| F
```

## Date role example

```mermaid
flowchart LR
    D[dim_date] -->|Active: order_date| F[fact_sales]
    D -.->|Inactive: ship_date| F
    D -.->|Inactive: delivery_date| F
```

**Recall:** one dimension can represent multiple business roles. Alternative relationships can be semantically valid while inactive to preserve one unambiguous default filter path.