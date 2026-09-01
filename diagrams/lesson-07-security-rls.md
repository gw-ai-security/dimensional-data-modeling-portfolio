# Lesson 07 — Security and RLS

Original Mermaid reconstruction of the security concepts documented in `docs/theory/lesson_07_security_rls.md`.

## Security levels

```mermaid
flowchart LR
    S[Security] --> T[Table-Level
protect entire table]
    S --> C[Column-Level
protect sensitive columns]
    S --> R[Row-Level Security
restrict visible rows]
```

## Dynamic RLS filter path

```mermaid
flowchart LR
    U[Current report user] --> P[USERPRINCIPALNAME]
    P --> Role[Dynamic role expression]
    Role --> Sec[Security / User mapping table]
    Sec -->|security filter| D[Dimension with authorization attribute]
    D -->|dimension filter| F[Fact table(s)]
```

## Security design workflow

```mermaid
flowchart TD
    A[Understand security requirement] --> B[Identify protected business scope]
    B --> C[Locate authorization attribute]
    C --> D{Static or Dynamic RLS?}
    D --> E[Define role / mapping]
    E --> F[Validate relationship + filter direction]
    F --> G[Test representative users / roles]
    G --> H[Reconcile restricted rows and totals]
```

## Security vs report filtering

```text
RLS           = What data may this user access at all?
Report filter = What allowed data does the user want to analyze now?
```

**Critical:** if the security filter cannot propagate from the security mapping through the model, RLS can fail and expose unintended data.