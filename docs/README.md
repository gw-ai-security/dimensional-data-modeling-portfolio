# Documentation Map

> **Status: FINAL.** Theory, guided implementation, runtime validation and independent audit are complete.

## Theory

[`theory/`](theory/README.md) contains the seven lesson references completed through video study and Active Recall.

## Foundation summaries

- [`01_modeling_fundamentals.md`](01_modeling_fundamentals.md)
- [`02_schema_patterns.md`](02_schema_patterns.md)
- [`03_relationships.md`](03_relationships.md)

## Capstone evidence

| File | Purpose | Status |
|---|---|---|
| [`04_source_model_assessment.md`](04_source_model_assessment.md) | source inventory and starting risks | ✅ |
| [`05_grain_analysis.md`](05_grain_analysis.md) | final grain matrix | ✅ |
| [`06_dimensions.md`](06_dimensions.md) | dimension design and validation | ✅ |
| [`07_facts.md`](07_facts.md) | fact design and validation | ✅ |
| [`08_semantic_measures.md`](08_semantic_measures.md) | measure catalog and reconciliation | ✅ |
| [`09_security.md`](09_security.md) | Dynamic RLS implementation and tests | ✅ |
| [`10_architecture_decisions.md`](10_architecture_decisions.md) | decisions and trade-offs | ✅ |
| [`11_lessons_learned.md`](11_lessons_learned.md) | theory + implementation lessons | ✅ |
| [`12_final_audit.md`](12_final_audit.md) | final runtime and no-tutorial audit | ✅ |

## Validation evidence

`../tests/` contains the completed release-style validation set:

```text
baseline_metrics.md
relationship_validation.md
reconciliation_tests.md
final_validation.md
```

## Evidence chain

```text
Theory
→ Active Recall
→ source assessment
→ dimensional implementation
→ grain / metric validation
→ debugging
→ RLS runtime tests
→ business report
→ independent no-tutorial audit
→ closure
```

The source-controlled PBIP/TMDL/PBIR plus repository-native diagrams represent the final implementation state.