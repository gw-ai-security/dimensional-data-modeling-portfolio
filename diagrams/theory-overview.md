# Complete Theory Overview

This diagram summarizes the complete pre-project theory path documented in `docs/theory/`.

```mermaid
flowchart TD
    A[Why Data Modeling Matters] --> B[Facts & Dimensions]
    B --> C[Star / Snowflake / Galaxy]
    C --> D[Relationships]
    D --> E[Special Dimensions]
    E --> F[Grain]
    F --> G[Multiple Facts]
    G --> H[Security / RLS]
    H --> I[Nightmare Hands-on Project]
```

## Working model

```mermaid
flowchart TD
    R[Understand business requirement] --> G[State Grain]
    G --> FD[Identify Facts & Dimensions]
    FD --> S[Design simple Star / Galaxy structure]
    S --> K[Validate Keys & Cardinality]
    K --> FP[Validate Filter Paths]
    FP --> MF{Multiple Facts?}
    MF -->|Same event/grain| A[Append or Merge when justified]
    MF -->|Different event/grain| SD[Shared Dimensions]
    A --> V[Validate Measures]
    SD --> V
    V --> SEC[Apply required Security / RLS]
    SEC --> REC[Reconcile rows and totals]
```

## Completion boundary

```text
Theory lessons 1–7     ✅ complete
Active Recall          ✅ complete
Power BI capstone      ⬜ not started
Independent audit      ⬜ not started
```

The next step is implementation evidence, not additional theory consumption.