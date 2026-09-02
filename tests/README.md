# Validation Evidence

This directory separates recorded model evidence from release checks that still require Power BI Desktop runtime validation.

## Current status

**Guided implementation built; validation partially complete.**

Recorded evidence includes protected metrics, a diagnosed order-process fan-out defect and source-controlled relationship/measure/RLS definitions. Final runtime smoke tests and the independent no-tutorial audit remain open.

## Validation categories

1. [`baseline_metrics.md`](baseline_metrics.md) — recorded business/control values.
2. [`relationship_validation.md`](relationship_validation.md) — final relationship inventory and open runtime checks.
3. [`reconciliation_tests.md`](reconciliation_tests.md) — before/after QA findings.
4. [`final_validation.md`](final_validation.md) — release gate.

## Evidence rule

A source-controlled definition proves implementation. It does **not** by itself prove Power BI runtime behavior.

```text
Implement
→ inspect source
→ refresh
→ reconcile
→ test interactions/security
→ capture evidence
→ release claim
```

The local source workbook is not in Git, so recorded numeric baselines include their provenance and are not presented as independently reproducible by GitHub alone.