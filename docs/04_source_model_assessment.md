# 04 — Source Model Assessment

> **Status: next active template.** The theory phase is complete. This is the first documentation artifact to fill when the guided Nightmare project begins.

## Purpose

Capture the starting state **before redesigning anything**. The goal is to understand business meaning, grain, relationships and structural risks before making changes.

## Assessment table

| Source table | Business meaning | Initial grain hypothesis | Candidate role | Observed issue | Decision |
|---|---|---|---|---|---|
| | | | | | |

## Questions to answer

For every source table:

- What business process/entity does it represent?
- What does exactly one row represent?
- Is it an entity/context table, event/fact table, mapping/bridge table or other supporting table?
- Which keys are expected to be unique?
- Which identifiers repeat and why?
- Which columns appear dimensional despite being embedded in facts?
- Are there mixed or incompatible grains inside one table?

For the model as a whole:

- Which relationships are `1:*`, `1:1` or `*:*`?
- Are any direct fact-to-fact relationships present?
- Are there bidirectional filters or ambiguous active paths?
- Are role-playing relationships already present?
- Which facts appear to share dimensions?
- Which data-quality issues could invalidate cardinality?
- Which business totals must be captured as baselines before remodeling?
- Is there an existing security/RLS path that must be preserved or redesigned?

## Before-state evidence required

- [ ] source table inventory
- [ ] initial business meaning for each table
- [ ] initial grain hypothesis for each fact-like table
- [ ] candidate Fact / Dimension / Bridge classification
- [ ] before-state model screenshot
- [ ] relationship/cardinality risk list
- [ ] duplicate-key/data-quality risk list
- [ ] baseline metrics recorded in `tests/baseline_metrics.md`

## Rule

Do not start restructuring tables simply because the target pattern is already known from the tutorial. The source assessment must document **why** each later modeling decision is needed.