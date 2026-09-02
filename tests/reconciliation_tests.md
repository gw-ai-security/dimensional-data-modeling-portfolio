# Reconciliation Tests

> **Status: key QA findings recorded; final post-PR runtime pass still open.**

| Check | Before / failure state | Final design / expected state | Result |
|---|---:|---:|---|
| Total Sales | 526,643.91 | 526,643.91 | recorded stable baseline |
| Distinct Orders | 80 | 80 | recorded stable baseline |
| Order Process grain | 97 rows / 80 Orders after naive child merges | 80 rows / 80 Orders | query hardened; runtime recheck pending |
| Sales Date relationship | headline Sales works; Sales Trend blank | Date-normalized fact should filter through `dim_date` | source fix implemented; visual recheck pending |
| Product Category | unexplained `(Blank)` | explicit `Unmapped` member, facts retained | source fix implemented; visual recheck pending |
| Sales vs Target | Target visible while Sales date path failed | both values visible at common time grain | source fix implemented; visual recheck pending |

## Failure rule

A variance is accepted only when it is explained by an intentional business rule or diagnosed defect. Cosmetic report filtering is not a reconciliation fix.

## Strongest debugging evidence

The Order Process case demonstrates the full loop:

```text
Observe: 97 rows for 80 Orders
→ Hypothesis: one-to-many child merges fan out Orders
→ Isolate: Shipments / Payments / Invoices
→ Fix: aggregate business milestones before join
→ Verify target grain: one row per Order
→ Prevent: row-count + distinct-key checks after future merges
```