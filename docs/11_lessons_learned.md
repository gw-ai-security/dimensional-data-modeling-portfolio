# 11 — Lessons Learned

## Theory-phase lessons — completed

### 1. A working report is not proof of a correct model

Relationship mistakes can produce plausible output without obvious technical errors. Validation must focus on semantics and expected business results, not only on whether visuals render.

### 2. Simplicity is a modeling feature

The course favors a star schema by default because direct Dimension → Fact relationships are easier to reason about. Extra normalization, relationships or bidirectional filtering need a concrete reason.

### 3. Data quality is part of relationship design

A duplicate key in a supposed dimension prevents a clean `1:*` relationship. Cardinality problems may originate upstream in the data rather than in the relationship dialog.

### 4. Filter direction is a semantic decision

`Both` is not a generic fix for missing filtering. It can create unexpected propagation and ambiguous paths. The default mental model is Dimension → Fact.

### 5. Inactive does not mean incorrect

An inactive relationship may be a deliberate alternative semantic path, especially for role-playing dimensions. The objective is one clear default path while still supporting alternative roles when needed.

### 6. Dimensions should carry analytical context

Descriptive attributes embedded in a fact should be reviewed. Coherent attributes can form a normal dimension; suitable low-level heterogeneous flags can be bundled into a Junk Dimension.

### 7. Grain comes before aggregation

The key question is: **What does exactly one row represent?**

A table can be at one grain while a measure is recorded at another. Higher-grain values repeated across lower-grain rows can produce silent double counting.

### 8. Grain comes before combining facts

The correct operation depends on business event, grain and shape:

```text
Same event + same grain + compatible shape → Append
Same grain + 1:1 complementary data       → Merge when justified
Different grain / event                    → Keep separate + Shared Dimensions
```

### 9. Shared context is preferable to direct fact coupling

When multiple facts need common analytical context, shared dimensions provide the connection rather than direct fact-to-fact many-to-many relationships.

### 10. Cross-fact comparison is limited by the coarser source

A measure cannot truthfully be displayed at a lower/finer grain than the level at which it was recorded. Daily Sales can be rolled up to monthly Budget; monthly Budget cannot become daily without an explicit allocation assumption.

### 11. Security is part of model topology

Dynamic RLS depends on a valid filter path:

```text
Current User
→ Security Mapping
→ Dimension
→ Fact
```

A wrong relationship or filter direction can prevent the restriction from propagating and can become a security leak.

### 12. `USERPRINCIPALNAME()` identifies the user; it is not the whole security model

Dynamic RLS requires identity + role/filter rule + security mapping + valid relationship propagation.

### 13. Access filtering and analytical filtering are different

```text
RLS           = what the user is allowed to see
Report filter = what the user chooses to analyze inside allowed data
```

### 14. Security must be requirements-driven and tested

Do not randomly apply access rules. Understand the requirement, choose the correct attachment point in the model, and test representative users/roles against expected restricted rows and totals.

## Corrections that improved the mental model

The Active Recall process corrected several useful misconceptions:

- Bidirectional filtering ≠ ambiguity.
- Inactive relationship ≠ broken relationship or snowflake split.
- Multiple Date roles ≠ many-to-many.
- `Junk Dimension`, not `Chunk Dimension`.
- Order-Line grain is finer than Order grain.
- Security levels are Table / Column / Row.
- `USERPRINCIPALNAME()` identifies identity; it does not directly filter every fact by itself.

See `learning/confusion_log.md` for the complete record.

## Capstone lessons — pending

The next entries must come from actual Nightmare-project implementation evidence: concrete failures, debugging traces, trade-offs, validation results and fixes. Theory-derived claims should not be duplicated here as if they were implementation experience.