# 04 — Source Model Assessment

> **Status: active.** The Nightmare PBIP project has been initialized from the source workbook. This is now the current implementation document and must be completed before any redesign.

## Purpose

Capture the starting state **before redesigning anything**. The goal is to understand business meaning, grain, relationships and structural risks before making changes.

## Environment checkpoint

- [x] Power BI Project saved in PBIP/TMDL format
- [x] PBIP project committed under `model/nightmare-data-model/`
- [x] course dataset excluded from Git and documented as local-only source data
- [x] local workbook source repointed to `<repository-root>/local-data/dataset.xlsx`
- [x] source model refreshed successfully after repointing
- [ ] before-state Model View screenshot stored as project evidence

The committed TMDL now points to the repository-local ignored workbook location on the current development machine (`C:\dev\dimensional-data-modeling-portfolio\local-data\dataset.xlsx`). This path is intentionally local/machine-specific; the workbook itself remains excluded from Git.

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
- [ ] before-state model screenshot stored in `screenshots/before/`
- [ ] relationship/cardinality risk list
- [ ] duplicate-key/data-quality risk list
- [ ] baseline metrics recorded in `tests/baseline_metrics.md`

## Rule

Do not start restructuring tables simply because the target pattern is already known from the tutorial. The source assessment must document **why** each later modeling decision is needed.
