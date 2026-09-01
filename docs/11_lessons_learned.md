# 11 — Lessons Learned

## Current lessons from the theory block

### 1. A working report is not proof of a correct model

Relationship mistakes may create plausible output without obvious technical errors. Validation must therefore focus on semantics and expected business results, not only on whether visuals render.

### 2. Simplicity is a modeling feature

The course favors a star schema by default because direct dimension-to-fact relationships make the model easier to reason about. Extra normalization/relationships need a concrete reason.

### 3. Data quality is part of relationship design

A duplicate key in a supposed dimension prevents a clean `1:*` relationship. Cardinality problems may therefore originate upstream in the data rather than in the relationship dialog itself.

### 4. Filter direction is a semantic decision

`Both` is not a generic fix for missing filtering. It can create unexpected propagation and ambiguous paths. The default mental model is Dimension → Fact.

### 5. Shared context is preferable to direct fact coupling

When multiple facts need common analytical context, shared dimensions provide the connection rather than a direct fact-to-fact many-to-many relationship.

## Capstone lessons

Add concrete failures, debugging evidence, trade-offs and fixes here once implementation starts.
