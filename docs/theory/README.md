# Theory Curriculum — Data Modeling in Power BI

> Primary source: the user-provided transcript of **Data with Baraa — Data Modeling in Power BI Full Course**. These notes paraphrase the course in original wording and structure; they do not reproduce the transcript verbatim.

This folder contains the complete **theory block before the Nightmare hands-on project**. The theory is split into seven lessons that follow the order of the transcript.

## Lesson map

| Lesson | Transcript range | Topic | Learning status |
|---|---:|---|---|
| [01](lesson_01_modeling_foundations.md) | 00:00:00–00:18:35 | Why modeling matters; data model components; facts vs dimensions | ✅ Completed |
| [02](lesson_02_schema_patterns.md) | 00:18:36–00:27:42 | Star, snowflake and galaxy schemas | ✅ Completed |
| [03](lesson_03_relationships.md) | 00:27:43–01:07:37 | Relationships, cardinality, filtering, ambiguity, active/inactive relationships | ✅ Completed |
| [04](lesson_04_special_dimensions.md) | 01:07:38–01:34:28 | Dimensions hidden in facts, junk dimensions, role-playing dimensions | ✅ Completed |
| [05](lesson_05_grain.md) | 01:34:29–01:41:27 | Grain at table and column/measure level | ✅ Completed |
| [06](lesson_06_multiple_facts.md) | 01:41:28–02:08:57 | Multiple facts, append/merge decisions, shared dimensions | ✅ Completed |
| [07](lesson_07_security_rls.md) | 02:08:58–02:40:07 | Table/column/row security; static and dynamic RLS | ✅ Completed |

The guided **Nightmare project starts at approximately 02:40:08** in the full-course transcript. Project implementation evidence belongs in the model/project documentation, not in these theory notes.

## Learning method

A lesson was not marked complete merely because notes existed. The completion loop used throughout the theory phase was:

```text
Watch
→ Explain from memory
→ Active Recall
→ Identify misconception
→ Correct and re-test
→ Mark complete
```

The detailed recall record is in [`../../learning/active_recall.md`](../../learning/active_recall.md), with resolved misconceptions in [`../../learning/confusion_log.md`](../../learning/confusion_log.md).

## Theory completion checkpoint

The theory phase now covers the complete pre-project course sequence:

```text
Why Modeling Matters
→ Facts & Dimensions
→ Star / Snowflake / Galaxy
→ Relationships
→ Special Dimensions
→ Grain
→ Multiple Facts
→ Security / RLS
```

Key gates passed through recall include:

- explaining why star schema and clear filter paths reduce model complexity;
- distinguishing cardinality from filter direction and ambiguity;
- distinguishing active/inactive relationships from many-to-many modeling;
- recognizing extracted, junk and role-playing dimensions;
- stating table grain and detecting higher-grain measures repeated at lower grain;
- choosing Append vs Merge vs separate facts based on grain/event/shape;
- comparing facts only at a grain both understand;
- explaining static vs dynamic RLS and why security depends on relationship/filter topology.

## Security checkpoint

The final theory lesson was completed after recall of:

- **Table-Level Security** — protect an entire table;
- **Column-Level Security** — protect specific sensitive columns;
- **Row-Level Security (RLS)** — restrict which rows a user may see;
- **Static RLS** — fixed role/filter rules, suitable for smaller scenarios;
- **Dynamic RLS** — user-to-scope mappings stored in a security table;
- `USERPRINCIPALNAME()` — identifies the current report user for the dynamic mapping;
- security filters must propagate through valid relationships into the analytical model;
- RLS must be validated with representative users/roles and expected restricted totals.

## Next phase

Theory consumption now stops. The next evidence level is the guided Nightmare implementation:

```text
Theory complete
→ inspect the source model
→ build in Power BI
→ validate each modeling decision
→ capture evidence
→ reconcile metrics
→ independent audit
```

See [`../../PROJECT_PLAN.md`](../../PROJECT_PLAN.md).

## Claim boundary

These files prove structured theory study and Active Recall, not production implementation. Practical claims will only be added after the Nightmare project is built and validated in Power BI.