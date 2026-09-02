# 11 — Lessons Learned

## Theory lessons

The theory phase established the core mental models: Star Schema by default, Dimension → Fact filtering, grain before aggregation, separate facts for different events, active/inactive role relationships, and requirements-driven RLS.

The detailed misconception record remains in `learning/confusion_log.md`.

## Capstone lessons

### 1. A successful Merge can still be wrong

Power Query can complete a join without error while silently changing business grain. The strongest example was `fact_order_process`: a naive child-event merge yielded **97 rows for 80 Orders**. The fix was not a DAX/report workaround; child events were reduced to explicit milestones before joining. Final validation confirmed **80 rows / 80 Orders**.

### 2. Protect totals and row identity together

Checking only Total Sales is insufficient. Row count, distinct business-key count and metric totals answer different QA questions. A model can preserve a total while still corrupting row identity.

### 3. Date types are part of relationship semantics

The first Business Overview could show Total Sales while Sales Trend was blank. The measure was correct; the Date relationship needed compatible Date grain. Normalizing the fact date fixed the semantic path rather than masking the problem in the visual.

### 4. Do not hide orphaned dimension values

An unexplained `(Blank)` Product Category is a referential-integrity signal. Filtering it out would conceal the problem. The final design uses an explicit Unmapped Product member so fact rows remain visible and totals remain reconcilable.

### 5. TMDL improves reviewability but does not replace runtime validation

PBIP/TMDL makes Power Query, measures, roles and relationships inspectable in Git. It still requires Power BI Desktop Refresh, Model View and `View As` validation before release claims are made.

### 6. RLS source code is not security proof

A role can be syntactically valid and still be semantically wrong. Final security evidence came from representative `View As` tests and expected Region-scoped behavior, not from the existence of the DAX expression alone.

### 7. Cross-fact KPIs need common business grain

Sales Targets are period/global data. Regional RLS on Sales does not create Region-level Targets. A KPI can be mathematically correct and still be semantically misleading if numerator and denominator do not share the same business scope.

### 8. Simple reporting is sufficient when the model is the deliverable

The Business Overview is deliberately small. Its job is to prove that dimensions filter measures predictably and that shared-date/cross-fact paths work. Additional visual complexity would not strengthen the core Data Modeling evidence.

### 9. Independent explanation is a different skill from tutorial reproduction

The project became complete only after the model could be explained from business event → grain → dimension/fact design → relationships → measures → reconciliation → security without following the instructor step by step.

## Final learning outcome

The strongest evidence from this project is not the final diagram alone. It is the ability to identify why a technically valid model can still be semantically wrong, diagnose the failure at the correct layer, and prove the correction with row counts, distinct keys, measures, filter paths and runtime behavior.

See [`12_final_audit.md`](12_final_audit.md) for project closure.