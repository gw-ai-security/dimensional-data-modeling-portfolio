# 03 — Relationships, Cardinality and Filter Direction

> Source basis: Data with Baraa Data Modeling full-course transcript. Detailed lesson evidence: [`theory/lesson_03_relationships.md`](theory/lesson_03_relationships.md).

## 1. Why relationships matter

A model can render reports without obvious technical errors and still produce wrong numbers if relationships are incorrect. Relationship errors can therefore become **silent errors**: plausible-looking output with incorrect semantic behavior.

A relationship is more than a line between tables. It defines a contract around:

- key columns used to match rows;
- cardinality;
- filter direction;
- active/inactive state.

## 2. Merge vs Relationship

### Merge

Use Power Query merge in the course scenarios when two inputs describe the same business object / compatible logical row and should become one analytical table.

### Relationship

Keep tables separate when they represent different analytical roles, such as a Dimension providing context to a Fact recording events.

This is the course decision framing for the demonstrated Power BI scenarios rather than a universal database-design law.

## 3. Cardinality

Cardinality is driven by key uniqueness:

```text
ONE  (1) = relationship key is unique on that side
MANY (*) = relationship key may repeat on that side
```

The standard star-schema pattern is:

```text
Dimension 1 → * Fact
```

A duplicate key on the intended Dimension `1` side is therefore a data-quality/model-preparation problem, not something to hide by forcing a relationship setting.

## 4. Filter direction

The course default is single-direction filtering:

```text
Dimension → Fact
```

The Dimension supplies analytical context; the Fact supplies events and measures evaluated under that context.

Bidirectional filtering (`Both`) is not a generic fix. It can allow filters to travel back through Facts and influence other Dimensions, increasing complexity and potentially contributing to multiple active paths.

## 5. Ambiguity

**Bidirectional filtering and ambiguity are different concepts.**

- Bidirectional filtering describes how a filter may cross a relationship.
- Ambiguity means more than one active filter path exists between parts of the model.

Example:

```text
Customer → Sales
Customer → Store → Sales
```

A Customer filter can reach Sales by two routes, creating competing semantics.

## 6. Active and inactive relationships

- **Active** — used automatically as a normal filter path.
- **Inactive** — semantically valid relationship that exists but is not used automatically.

Inactive does **not** mean broken, `1:1`, or snowflaked.

A role-playing Date pattern can contain:

```text
dim_date[date] 1 → * fact_sales[order_date]   ACTIVE
dim_date[date] 1 - - * fact_sales[ship_date] INACTIVE
```

Both relationships remain `1:*`; multiple Date roles do not imply many-to-many cardinality.

## 7. Healthy relationship checklist

- [x] Dimension-side uniqueness understood as prerequisite for `1:*`
- [x] Fact-side foreign key repetition understood
- [x] Dimension → Fact established as default filter direction
- [x] Bidirectional filtering distinguished from ambiguity
- [x] Active/inactive semantics understood
- [x] Role-playing relationships distinguished from many-to-many
- [x] Direct fact-to-fact relationships recognized as a modeling risk

Project-specific validation remains pending until the Nightmare model is implemented.

## Theory evidence status

- Concept documented: ✅
- Active Recall completed: ✅
- Misconceptions corrected and re-tested: ✅
- Capstone implementation: ⬜ not started
- Project relationship validation: ⬜ not started

See `learning/active_recall.md` and `learning/confusion_log.md` for the recall/correction record.