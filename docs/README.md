# Documentation Map

This repository separates **theory evidence** from **capstone implementation evidence** so that completed learning is not confused with work that has not yet been built.

## 1. Theory reference — complete

[`theory/`](theory/README.md) contains the full lesson-by-lesson pre-project theory reference:

```text
Lesson 01 — Modeling Foundations
Lesson 02 — Schema Patterns
Lesson 03 — Relationships
Lesson 04 — Special Dimensions
Lesson 05 — Grain
Lesson 06 — Multiple Facts
Lesson 07 — Security / RLS
```

All seven lessons are completed through video study + Active Recall. Related visual summaries are in `../diagrams/`.

## 2. Concise foundation summaries

These files are shorter reference summaries for core concepts:

- [`01_modeling_fundamentals.md`](01_modeling_fundamentals.md)
- [`02_schema_patterns.md`](02_schema_patterns.md)
- [`03_relationships.md`](03_relationships.md)

They do not replace the detailed lesson documents in `theory/`.

## 3. Nightmare capstone evidence — ready, not yet implemented

The following files are evidence templates that will be populated from the actual Power BI Nightmare project:

| File | Purpose | Current state |
|---|---|---|
| [`04_source_model_assessment.md`](04_source_model_assessment.md) | source inventory, business meaning, before-state risks | ▶ next active |
| [`05_grain_analysis.md`](05_grain_analysis.md) | table/measure grain matrix and validation | ▶ next active |
| [`06_dimensions.md`](06_dimensions.md) | implemented Dimension design/evidence | ⬜ pending implementation |
| [`07_facts.md`](07_facts.md) | implemented Fact design/evidence | ⬜ pending implementation |
| [`08_semantic_measures.md`](08_semantic_measures.md) | measures and reconciliation evidence | ⬜ pending implementation |
| [`09_security.md`](09_security.md) | RLS requirement, filter path and tests | ⬜ pending implementation |
| [`10_architecture_decisions.md`](10_architecture_decisions.md) | modeling decisions and trade-offs | ⬜ pending implementation |
| [`11_lessons_learned.md`](11_lessons_learned.md) | theory lessons + later implementation lessons | theory ✅ / capstone ⬜ |

## 4. Validation evidence

Project validation belongs in `../tests/`:

```text
baseline_metrics.md
relationship_validation.md
reconciliation_tests.md
final_validation.md
```

A template is not evidence of a passed test. Results are added only after execution against the real Power BI model.

## 5. Claim boundary

Current claim:

```text
Theory + Active Recall          ✅ complete
Original learning diagrams      ✅ complete
Nightmare Power BI model         ⬜ not started
Capstone validation evidence     ⬜ not started
Independent no-tutorial audit    ⬜ not started
```

This separation is intentional and should be preserved throughout the project.