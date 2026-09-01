# Diagrams

Repository-native Mermaid diagrams make the modeling concepts inspectable directly on GitHub. These are original reconstructions of the learned concepts rather than copied course graphics.

## Theory diagrams — complete

- [`theory-overview.md`](theory-overview.md) — complete pre-project theory path and working decision model
- [`lesson-01-foundations.md`](lesson-01-foundations.md) — why modeling matters; facts vs dimensions
- [`lesson-02-schema-patterns.md`](lesson-02-schema-patterns.md) — star, snowflake and galaxy schemas
- [`lesson-03-relationships.md`](lesson-03-relationships.md) — cardinality, filter direction, ambiguity
- [`lesson-04-special-dimensions.md`](lesson-04-special-dimensions.md) — extracted, junk and role-playing dimensions
- [`lesson-05-grain.md`](lesson-05-grain.md) — table grain, measure grain and double counting
- [`lesson-06-multiple-facts.md`](lesson-06-multiple-facts.md) — Append vs Merge vs separate facts; shared dimensions
- [`lesson-07-security-rls.md`](lesson-07-security-rls.md) — security levels, Dynamic RLS and security filter path

## Capstone diagrams — pending implementation

The Nightmare project will add evidence-backed diagrams only after the corresponding Power BI states actually exist:

- source model before-state;
- source/problem relationship map;
- grain/fact/dimension design map;
- target dimensional model;
- final star/galaxy schema;
- role-playing relationship view;
- final RLS/security filter path.

## Diagram rules

- diagrams must represent an actual concept, decision or implemented model state;
- project diagrams must match the Power BI artifact and documentation;
- relationship direction/cardinality must be labeled where material;
- avoid copied course screenshots/graphics; reconstruct diagrams in original form;
- do not imply capstone implementation before model evidence exists.