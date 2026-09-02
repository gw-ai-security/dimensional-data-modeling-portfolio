# Theory Curriculum — Data Modeling in Power BI

> Primary source: the user-provided transcript of **Data with Baraa — Data Modeling in Power BI Full Course**. These notes paraphrase the course in original wording and structure; they do not reproduce the transcript verbatim.

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

The guided Nightmare project begins after this theory block. The theory files remain historical learning evidence; they are not rewritten to pretend the later implementation was independently derived.

## Learning method

```text
Watch
→ Explain from memory
→ Active Recall
→ Identify misconception
→ Correct and re-test
→ Mark complete
```

Detailed recall evidence is in `../../learning/active_recall.md` and `../../learning/confusion_log.md`.

## Current project context

The guided Nightmare implementation has now been built and source-controlled. The current project gate is no longer “start the capstone”; it is:

```text
Theory complete
→ guided implementation complete
→ final Power BI runtime validation
→ representative RLS tests
→ final screenshots
→ independent no-tutorial audit
```

See [`../../PROJECT_PLAN.md`](../../PROJECT_PLAN.md).

## Claim boundary

These theory files prove structured study and Active Recall. Practical implementation/validation evidence lives in `../`, `../../model/`, `../../tests/` and `../../screenshots/`.