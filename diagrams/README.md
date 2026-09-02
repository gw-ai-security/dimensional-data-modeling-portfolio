# Diagrams

Repository-native Mermaid diagrams make theory and implementation states inspectable on GitHub. They are original reconstructions rather than copied course graphics.

## Theory diagrams — complete

- [`theory-overview.md`](theory-overview.md)
- [`lesson-01-foundations.md`](lesson-01-foundations.md)
- [`lesson-02-schema-patterns.md`](lesson-02-schema-patterns.md)
- [`lesson-03-relationships.md`](lesson-03-relationships.md)
- [`lesson-04-special-dimensions.md`](lesson-04-special-dimensions.md)
- [`lesson-05-grain.md`](lesson-05-grain.md)
- [`lesson-06-multiple-facts.md`](lesson-06-multiple-facts.md)
- [`lesson-07-security-rls.md`](lesson-07-security-rls.md)

## Final capstone diagram

- [`capstone-progress.md`](capstone-progress.md) — finalized star/galaxy semantic model, role-playing relationships, Dynamic RLS path and grain-hardening QA.

## Evidence strategy

The original chaotic Model View is committed at `../screenshots/before/before.png`. The final state is represented by the source-controlled PBIP/TMDL/PBIR, the final Mermaid model and the completed validation artifacts in `../tests/`.

A duplicate after-state PNG is optional presentation evidence, not a project-closure requirement.

## Rules

- diagrams must match committed TMDL;
- technical Auto Date artifacts are not presented as business dimensions;
- inactive role relationships are shown as alternatives;
- runtime correctness is proven by validation evidence, not by a diagram alone.