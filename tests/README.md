# Validation Evidence

> **Status: COMPLETE — final Power BI runtime, reconciliation and security checks passed.**

This directory contains the release-style evidence for the finalized Nightmare dimensional-modeling project.

## Validation set

1. [`baseline_metrics.md`](baseline_metrics.md) — protected business/control values.
2. [`relationship_validation.md`](relationship_validation.md) — final relationship inventory and runtime checks.
3. [`reconciliation_tests.md`](reconciliation_tests.md) — failure states, fixes and final results.
4. [`final_validation.md`](final_validation.md) — completed project closure checklist.

## Evidence rule

A source-controlled definition proves implementation; final claims additionally require runtime validation.

```text
Implement
→ inspect source
→ refresh
→ reconcile
→ test relationships/interactions
→ test RLS
→ independent audit
→ release claim
```

All stages above are complete for this project.

The local source workbook is intentionally excluded from Git, so numeric references are recorded with provenance rather than presented as values GitHub can independently recompute.