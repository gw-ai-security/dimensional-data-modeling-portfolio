# Documentation Map

This repository separates **theory evidence** from **capstone implementation evidence** so completed learning is not confused with work that has not yet been built.

## 1. Theory reference — complete

[`theory/`](theory/README.md) contains the seven lesson-by-lesson theory references. All seven lessons are complete through video study and Active Recall. Related visual summaries are in `../diagrams/`.

## 2. Concise foundation summaries

- [`01_modeling_fundamentals.md`](01_modeling_fundamentals.md)
- [`02_schema_patterns.md`](02_schema_patterns.md)
- [`03_relationships.md`](03_relationships.md)

## 3. Nightmare capstone evidence — implementation in progress

**Current guided checkpoint: 03:45:58.**

| File | Purpose | Current state |
|---|---|---|
| [`04_source_model_assessment.md`](04_source_model_assessment.md) | source inventory, business meaning and starting risks | ✅ coarse investigation documented; screenshot/baseline follow-up open |
| [`05_grain_analysis.md`](05_grain_analysis.md) | grain matrix and header/detail decisions | ▶ active evidence; orders/order-lines/dimensions documented |
| [`06_dimensions.md`](06_dimensions.md) | implemented Dimension design/evidence | ▶ active; `dim_customer`, `dim_product`, `dim_order_flags` documented |
| [`07_facts.md`](07_facts.md) | implemented Fact design/evidence | ⬜ first fact is next guided step |
| [`08_semantic_measures.md`](08_semantic_measures.md) | measures and reconciliation evidence | ⬜ pending |
| [`09_security.md`](09_security.md) | RLS requirement, filter path and tests | ⬜ pending |
| [`10_architecture_decisions.md`](10_architecture_decisions.md) | modeling decisions and trade-offs | ▶ active; first six decisions recorded |
| [`11_lessons_learned.md`](11_lessons_learned.md) | theory + later implementation lessons | theory ✅ / capstone ongoing |

## 4. Current model evidence

The PBIP/TMDL artifact under `../model/nightmare-data-model/` currently contains:

```text
01_Stage
├── orders
├── channels
└── source/reference queries

02_Dimensions
├── dim_customer
├── dim_product
└── dim_order_flags

03_Facts
└── first analytical fact not yet created at 03:45:58
```

See [`../diagrams/capstone-progress.md`](../diagrams/capstone-progress.md) for the current transformation map.

## 5. Validation evidence

Project validation belongs in `../tests/`:

```text
baseline_metrics.md
relationship_validation.md
reconciliation_tests.md
final_validation.md
```

A template is not evidence of a passed test. In particular, the protected sales baseline has not yet been documented at the current checkpoint and must be recorded as the first fact is built.

## 6. Claim boundary

Current claim:

```text
Theory + Active Recall                ✅ complete
Original theory diagrams              ✅ complete
PBIP/TMDL capstone                     ▶ in progress
Source/business investigation          ✅ coarse pass complete
Customer/Product dimensions            ✅ implemented
Order junk dimension                   ▶ built; final polish next
First analytical fact                  ⬜ next
Metric reconciliation                  ⬜ pending
RLS/final model validation              ⬜ pending
Independent no-tutorial audit           ⬜ pending
```

Documentation must continue to match the committed TMDL state rather than the instructor's completed solution.