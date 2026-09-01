# Theory Curriculum — Data Modeling in Power BI

> Primary source: the user-provided transcript of **Data with Baraa — Data Modeling in Power BI Full Course**. These notes paraphrase the course in original wording and structure; they do not reproduce the transcript verbatim.

This folder contains the complete **theory block before the Nightmare hands-on project**. The theory is split into seven lessons that follow the order of the transcript.

## Lesson map

| Lesson | Transcript range | Topic | Learning status |
|---|---:|---|---|
| [01](lesson_01_modeling_foundations.md) | 00:00:00–00:18:35 | Why modeling matters; data model components; facts vs dimensions | Completed |
| [02](lesson_02_schema_patterns.md) | 00:18:36–00:27:42 | Star, snowflake and galaxy schemas | Completed |
| [03](lesson_03_relationships.md) | 00:27:43–01:07:37 | Relationships, cardinality, filtering, ambiguity, active/inactive relationships | Completed |
| [04](lesson_04_special_dimensions.md) | 01:07:38–01:34:28 | Dimensions hidden in facts, junk dimensions, role-playing dimensions | Not yet studied |
| [05](lesson_05_grain.md) | 01:34:29–01:41:27 | Grain at table and column level | Not yet studied |
| [06](lesson_06_multiple_facts.md) | 01:41:28–02:08:57 | Multiple facts, append/merge decisions, shared/bridge dimensions | Not yet studied |
| [07](lesson_07_security_rls.md) | 02:08:58–02:40:07 | Table/column/row security; static and dynamic RLS | Not yet studied |

The **guided Nightmare project starts at approximately 02:40:08** in the full-course transcript. Project implementation evidence belongs in the separate model/project documentation, not in these theory notes.

## How to use these notes

The documentation can be read ahead, but a lesson is not marked as learned merely because its notes exist. The intended learning loop is:

```text
Watch → Explain from memory → Active Recall → Correct misunderstandings → Apply in the capstone
```

Each lesson therefore contains:

- the source-derived concepts and rules,
- the modeling rationale shown in the course,
- failure modes demonstrated or warned about in the transcript,
- a compact decision model,
- Active Recall questions.

## Current checkpoint

Lessons 1–3 are now completed. Lesson 3 was closed only after the Active Recall identified and corrected three specific misconceptions: bidirectional filtering vs ambiguity, inactive relationship semantics, and the cardinality of role-playing date relationships.

The next learning block starts with **Lesson 4 — Special Dimensions** at approximately `01:07:38`.

## Claim boundary

These files are **learning documentation**, not implementation evidence. Practical claims will only be added after the Nightmare project is built and validated in Power BI.
