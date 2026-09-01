# 07 — Fact Design

> **Status: planned template.** Facts have not yet been implemented in the capstone.

For each fact table, document the business process and grain before defining measures or relationships.

## Fact template

### Business process

What event/activity does this fact record?

### Grain

> One row represents ...

### Measures / numeric facts

| Column | Meaning | Additivity / caveat |
|---|---|---|
| | | |

### Dimension keys

| Foreign key | Dimension | Expected match behavior |
|---|---|---|
| | | |

### Baseline metrics to protect

| Metric | Baseline | Post-change | Expected | Result |
|---|---:|---:|---|---|
| | | | unchanged | |

### Validation

- [ ] row-count behavior understood
- [ ] grain preserved after transformation
- [ ] no accidental row multiplication
- [ ] dimension-key matches investigated
- [ ] business totals reconciled
