# Reconciliation Tests

> **Status: COMPLETE — final Power BI runtime pass reconciled the key business and modeling controls.**

| Check | Before / failure state | Final state | Result |
|---|---:|---:|---|
| Total Sales | 526,643.91 | 526,643.91 | PASS |
| Distinct Orders | 80 | 80 | PASS |
| Order Process grain | 97 rows / 80 Orders after naive child merges | 80 rows / 80 Orders | PASS |
| Sales Date relationship | headline Sales worked; Sales Trend blank | Date-normalized fact filters correctly through `dim_date` | PASS |
| Product Category | unexplained `(Blank)` | explicit `Unmapped` member, fact rows retained | PASS |
| Sales vs Target | Target visible while Sales date path failed | both values render at valid common time grain | PASS |
| Dynamic RLS | role existed only as source definition | representative `View As` scopes behave as expected | PASS |

## Failure rule

A variance is accepted only when explained by an intentional business rule or a diagnosed defect. Cosmetic report filtering is not a reconciliation fix.

## Strongest debugging evidence

The Order Process case demonstrates the full loop:

```text
Observe: 97 rows for 80 Orders
→ Hypothesis: one-to-many child merges fan out Orders
→ Isolate: Shipments / Payments / Invoices
→ Fix: aggregate business milestones before join
→ Verify: 80 rows / 80 Orders
→ Prevent: row-count + distinct-key checks after future merges
```

The Date and Unmapped Product cases add two further validation patterns: fix semantic-key compatibility at the model layer, and surface referential-integrity exceptions explicitly instead of hiding them in the report.