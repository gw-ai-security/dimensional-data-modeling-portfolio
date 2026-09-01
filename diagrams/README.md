# Diagrams

Repository-native Mermaid diagrams make modeling concepts and implemented project states inspectable directly on GitHub. These are original reconstructions rather than copied course graphics.

## Theory diagrams — complete

- [`theory-overview.md`](theory-overview.md) — complete pre-project theory path and working decision model
- [`lesson-01-foundations.md`](lesson-01-foundations.md) — why modeling matters; facts vs dimensions
- [`lesson-02-schema-patterns.md`](lesson-02-schema-patterns.md) — star, snowflake and galaxy schemas
- [`lesson-03-relationships.md`](lesson-03-relationships.md) — cardinality, filter direction, ambiguity
- [`lesson-04-special-dimensions.md`](lesson-04-special-dimensions.md) — extracted, junk and role-playing dimensions
- [`lesson-05-grain.md`](lesson-05-grain.md) — table grain, measure grain and double counting
- [`lesson-06-multiple-facts.md`](lesson-06-multiple-facts.md) — Append vs Merge vs separate facts; shared dimensions
- [`lesson-07-security-rls.md`](lesson-07-security-rls.md) — security levels, Dynamic RLS and security filter path

## Capstone diagrams — in progress

- [`capstone-progress.md`](capstone-progress.md) — evidence-backed transformation map through video checkpoint 03:45:58: customer/product dimensions, unified order staging, junk dimension and next `fact_sales` step

Still pending until corresponding implementation evidence exists:

- committed source-model before-state image;
- final relationship/problem map;
- complete fact/dimension design map;
- final star/galaxy schema;
- role-playing date relationship view;
- final RLS/security filter path.

## Diagram rules

- diagrams must represent an actual concept, decision or implemented model state;
- project diagrams must match the committed Power BI artifact and documentation;
- planned/not-yet-built objects must be visually and textually distinguished from implemented objects;
- relationship direction/cardinality must be labeled where material;
- avoid copied course screenshots/graphics; reconstruct diagrams in original form;
- do not imply final capstone completion before model and validation evidence exist.