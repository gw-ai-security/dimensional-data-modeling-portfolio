# 03 — Relationships, Cardinality and Filter Direction

> Source basis: Data with Baraa Data Modeling full-course transcript. This document captures the relationship block covered so far.

## 1. Why relationships matter

A model can render reports without obvious technical errors and still produce wrong numbers if relationships are incorrect. Relationship errors can therefore become **silent errors**: plausible-looking output with incorrect semantic behavior.

A relationship is more than a line between tables. It defines a contract around:

- the key columns used to match rows
- cardinality
- filter direction
- active/inactive state

## 2. Merge vs relationship

The course distinguishes physical table consolidation from semantic relationships.

### Merge

Use Power Query merge when two inputs belong to the same business object / same logical row and should become one table.

Example idea:

```text
Customer master + customer email attributes → one customer dimension
```

### Relationship

Keep tables separate when they represent different business concepts/events and should interact through the model.

Example:

```text
Customer dimension ↔ Sales fact
```

This is not an absolute database-design law; it is the course decision framing for the Power BI modeling scenarios shown.

## 3. Relationship key

Power BI needs columns that identify how rows match between tables.

Typical star-schema pattern:

```text
dim_customer.customer_id → fact_sales.customer_id
```

The dimension-side value identifies one business entity while the same identifier may repeat in the fact because one entity can participate in many events.

## 4. Cardinality

The course explains cardinality through uniqueness:

- **ONE (`1`)** — relationship-key values are unique on that side.
- **MANY (`*`)** — relationship-key values may repeat on that side.

### One-to-many (`1:*`)

The normal star-schema pattern:

```text
Dimension 1 → * Fact
```

Example: one customer row can relate to many sales rows.

### Many-to-one (`*:1`)

The same relationship read from the opposite direction.

### One-to-one (`1:1`)

Both sides contain unique relationship keys. When both tables also represent the same business object, the modeler should consider whether they should instead be combined.

### Many-to-many (`*:*`)

Both sides contain repeated relationship keys. The course treats this as a risky pattern that needs deliberate modeling rather than automatic acceptance.

A common anti-pattern is a direct relationship between two facts. A shared dimension is preferred when it correctly represents the business key/context.

## 5. Data quality and cardinality

A planned dimension `1` side cannot behave as `1` if duplicate keys exist.

Example:

```text
product_id
1
2
6
6   ← duplicate
```

If `product_id` is supposed to uniquely identify a product in the dimension, the duplicate is a data-quality/model-preparation problem. It should be investigated and corrected before relying on a `1:*` relationship.

This gives an important dependency chain:

```text
Data quality → Cardinality → Relationship behavior → Report result
```

## 6. Filter direction

The course default for a star schema is **single-direction filtering from dimension to fact**:

```text
Dimension 1 → * Fact
```

The dimension provides the filter context; the fact supplies the events and numeric values evaluated under that context.

## 7. Why bidirectional (`Both`) filtering is risky

If filters can travel back from facts into dimensions and across the model, one selection may unexpectedly filter other dimensions. This can make report behavior difficult to predict and can contribute to multiple filter paths.

The course recommendation is therefore to keep `Both` for cases where it is specifically required and understood rather than using it as a default.

## 8. Ambiguity

Ambiguity occurs when more than one active filter path can connect the same areas of the model.

Example:

```text
Customer → Sales
Customer → Store → Sales
```

A customer filter can reach sales through two routes. The engine then lacks one unambiguous semantic path.

## 9. Active and inactive relationships

- **Active relationship** — used automatically for filter propagation.
- **Inactive relationship** — exists in the model but is not used automatically.

An inactive relationship can prevent competing active paths and is also useful later for scenarios such as role-playing dimensions.

## 10. Healthy relationship checklist

For the course's default star-schema pattern, verify:

- [ ] dimension-side key is unique
- [ ] fact-side foreign key can repeat
- [ ] cardinality is `1:*`
- [ ] filter direction is Dimension → Fact
- [ ] no accidental direct fact-to-fact relationship
- [ ] no unexplained many-to-many relationship
- [ ] no competing active filter paths
- [ ] relationship behavior matches the business meaning

## Current evidence status

- Concept documented: ✅
- Active-recall checkpoint: 🟡 pending for this lesson
- Capstone implementation: ⬜ not started
- Relationship validation on project model: ⬜ not started
