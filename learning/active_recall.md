# Active Recall Question Bank

The goal is to answer these questions without reading the documentation first.

## Lesson 1 — Fundamentals

1. Why does one large flat table become problematic as the model grows?
2. Why is `one table → one report` a maintenance risk?
3. What are the three core components of a data model in the course framing?
4. What is the fundamental difference between a fact and a dimension?
5. In `Sales by Product Category`, what comes from the fact side and what comes from the dimension side?

### Minimum recall target

- repeated descriptive data → larger model / performance cost
- duplicated report logic → maintenance / inconsistent KPIs
- Tables + Relationships + Calculations
- Fact = what happened; Dimension = descriptive context
- Sales = fact/measure; Product Category = dimension/context

## Lesson 2 — Schema patterns

1. How is a star schema structured?
2. What is the normal filter direction in the course's star-schema model?
3. Why can a good model simplify DAX?
4. When would snowflaking a dimension be considered and what is the trade-off?
5. What makes a galaxy schema, and how should multiple facts be connected?

### Minimum recall target

- fact center, dimensions around it
- Dimension → Fact
- relationships/model structure carry semantic work
- snowflake only when justified; more relationships/complexity
- multiple facts share dimensions; no direct fact-to-fact relationship

## Lesson 3 — Relationships

1. What is the difference between Merge and Relationship in the course scenarios?
2. What does `ONE` mean in cardinality? What does `MANY` mean?
3. Why is `Dimension 1 → * Fact` the normal star-schema pattern?
4. What is the default filter direction?
5. Why is `Both` potentially problematic?
6. What is ambiguity?
7. What is the difference between active and inactive relationships?
8. Why does a duplicate product key break the intended `1:*` relationship?
9. What is the difference between bidirectional filtering and ambiguity?
10. Why can a relationship be intentionally inactive even when it is semantically correct?
11. If `fact_sales` contains both `order_date` and `ship_date`, how can one `dim_date` relate to both without creating many-to-many cardinality?

### Lesson 3 checkpoint — 2026-09-01

**Status: completed after correction and re-test.**

Correctly recalled:

- `dim_customer (1) → (*) fact_sales` when the customer key is unique in the dimension and repeated in the fact;
- single-direction filtering from Dimension → Fact as the normal star-schema default;
- direct fact-to-fact many-to-many should be avoided in favor of a shared dimension;
- ambiguity means multiple active filter paths;
- inactive relationships can represent valid alternative paths that are not used automatically.

Misconceptions identified and corrected:

1. **Bidirectional filtering vs ambiguity** — bidirectional filtering allows filters to travel both ways across a relationship; ambiguity is the existence of multiple active filter paths. Bidirectional filtering can contribute to ambiguity, but the concepts are not identical.
2. **Inactive relationship semantics** — an inactive relationship is not inherently `1:1`, not a broken relationship, and not a snowflake split. It is a valid relationship that is not the default active filter path.
3. **Role-playing date dimension** — `dim_date[date]` can relate `1:*` to both `fact_sales[order_date]` and `fact_sales[ship_date]`; one relationship may be active and the other inactive. Multiple date roles do not imply `*:*`.

## Review rule

A concept is not marked mastered merely because the answer looks familiar. I should be able to explain it, sketch it and apply it to an unfamiliar model.
