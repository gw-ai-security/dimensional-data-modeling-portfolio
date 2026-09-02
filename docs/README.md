# Documentation Map

This repository separates theory, implementation and validation evidence.

## Theory — complete

[`theory/`](theory/README.md) contains the seven lesson-by-lesson theory references. All seven lessons are complete through video study and Active Recall.

## Foundation summaries

- [`01_modeling_fundamentals.md`](01_modeling_fundamentals.md)
- [`02_schema_patterns.md`](02_schema_patterns.md)
- [`03_relationships.md`](03_relationships.md)

## Nightmare capstone — current state

| File | Purpose | Current state |
|---|---|---|
| [`04_source_model_assessment.md`](04_source_model_assessment.md) | source inventory, business meaning and starting risks | ✅ source assessment complete |
| [`05_grain_analysis.md`](05_grain_analysis.md) | final grain matrix | ✅ implementation grains documented |
| [`06_dimensions.md`](06_dimensions.md) | dimension design/evidence | ✅ six dimensions documented |
| [`07_facts.md`](07_facts.md) | fact design/evidence | ✅ six facts documented |
| [`08_semantic_measures.md`](08_semantic_measures.md) | measure catalog and recorded values | ✅ implemented; runtime release checks remain |
| [`09_security.md`](09_security.md) | dynamic RLS implementation and test plan | ▶ implemented; representative `View As` tests pending |
| [`10_architecture_decisions.md`](10_architecture_decisions.md) | modeling/QA decisions and trade-offs | ✅ current decisions recorded |
| [`11_lessons_learned.md`](11_lessons_learned.md) | theory + capstone lessons | ✅ current implementation lessons recorded |

## Validation evidence

`../tests/` now separates recorded evidence from still-open runtime checks:

```text
baseline_metrics.md
relationship_validation.md
reconciliation_tests.md
final_validation.md
```

The important distinction is:

```text
source-controlled implementation ≠ Power BI runtime proof
```

The final release gate therefore remains open until the latest PBIP state is refreshed/tested, RLS is exercised with representative users, final screenshots are committed and the independent audit is completed.