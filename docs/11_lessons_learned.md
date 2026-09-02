# 11 — Lessons Learned

## Theory lessons

The theory phase established the core mental models: Star Schema by default, Dimension → Fact filtering, grain before aggregation, separate facts for different events, active/inactive role relationships, and requirements-driven RLS.

The detailed misconception record remains in `learning/confusion_log.md`.

## Capstone lessons

### 1. A successful Merge can still be wrong

Power Query can complete a join without error while silently changing business grain. The strongest example in this project was `fact_order_process`: a naive child-event merge yielded 97 rows for 80 Orders. The fix was not a visual/DAX workaround; child events were reduced to explicit milestones before joining.

### 2. Protect totals and row identity together

Checking only Total Sales is insufficient. Row count, distinct business key count and metric totals answer different QA questions. A model can preserve a total while duplicating or losing row identity elsewhere.

### 3. Date types are part of relationship semantics

The first Business Overview could show Total Sales while Sales Trend was blank. The measure was correct; the Date relationship could not match DateTime values with time components to the shared Date key. Normalizing the fact date fixed the model path rather than the visual.

### 4. Do not hide orphaned dimension values in the report

An unexplained `(Blank)` Product Category is a referential-integrity signal. Filtering the visual would conceal the issue. The latest design uses an explicit Unmapped Product member so fact rows remain visible and totals stay reconcilable.

### 5. TMDL improves reviewability but does not replace runtime validation

PBIP/TMDL makes Power Query, measures, roles and relationships inspectable in Git. It also makes direct source edits possible. Those edits still require Power BI Desktop Refresh/Model View/`View As` smoke tests before release claims are made.

### 6. RLS source code is not security proof

A role can be syntactically present and still be semantically wrong. Security evidence requires representative user tests and restricted-total reconciliation.

### 7. Cross-fact KPIs need common business grain

Sales Targets are period/global data. Regional RLS on Sales does not magically create region-level Targets. A KPI can be mathematically valid and still be semantically misleading if numerator and denominator do not share the same business scope.

### 8. Simple reporting is enough to validate a strong model

The Business Overview is deliberately small. Its purpose is to demonstrate that dimensions filter measures predictably and that cross-fact/date paths work. More visuals would not increase the core modeling evidence.

## Current release lesson

The guided project is built, but the final evidence gate is intentionally still open until the latest PBIP refresh, RLS runtime tests, final screenshots and no-tutorial audit are complete. Keeping that distinction explicit is part of the engineering evidence.