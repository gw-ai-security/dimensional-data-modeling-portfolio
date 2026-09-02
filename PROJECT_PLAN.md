# Project Plan — Completed

## Goal

Build defensible evidence of dimensional data modeling competence through the full loop:

```text
Theory
→ guided implementation
→ validation/debugging
→ business proof
→ security testing
→ independent no-tutorial audit
```

**Final status: COMPLETE — 2026-09-02.**

## Phase A — Theory foundations ✅

All seven theory lessons were completed and checked through Active Recall.

## Phase B — Guided Nightmare implementation ✅

### B1 — Prepare and investigate

- [x] PBIP/TMDL project initialized
- [x] course workbook kept local-only and source repointed
- [x] Power Query groups created: `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`
- [x] source/business inventory documented
- [x] starting Model View committed at `screenshots/before/before.png`
- [x] header/detail and major source grains documented

### B2 — Dimensions

- [x] `dim_customer`
- [x] `dim_product`
- [x] `dim_order_flags`
- [x] `dim_geo`
- [x] `dim_campaign`
- [x] `dim_date`
- [x] final naming standardized
- [x] manual channel-mapping risk documented
- [x] explicit Unmapped Product member implemented and runtime-validated

### B3 — Facts

- [x] `fact_sales` — Order-Line grain
- [x] `fact_inventory` — Product × Month grain
- [x] `fact_campaign_spend` — Campaign × Date grain
- [x] `fact_promotion_coverage` — Campaign × Product grain
- [x] `fact_order_process` — one row per Order
- [x] `fact_sales_targets` — Period grain
- [x] order-process merge fan-out diagnosed and corrected through milestone aggregation

### B4 — Relationships / semantic layer

- [x] dimensions connected to facts; no direct fact-to-fact relationship
- [x] shared Date relationships validated
- [x] role-playing Order Process dates validated with active/inactive relationships
- [x] Ship-To/Bill-To geography roles validated
- [x] `_measures` table contains grain-aware business measures
- [x] `regional access` Dynamic RLS role implemented
- [x] `Business Overview` and `Model Validation` pages implemented

## Phase C — Final validation ✅

- [x] final Power BI Desktop Refresh completed successfully
- [x] Sales Trend validated after Date normalization
- [x] Sales vs Target validated at a compatible common time grain
- [x] Unmapped Product behavior validated
- [x] `fact_order_process` validated at 80 rows / 80 distinct Orders
- [x] relationship/cardinality/filter behavior smoke-tested
- [x] representative Dynamic RLS `View As` scenarios validated
- [x] restricted behavior reconciled against expected Region scopes
- [x] core business measures reconciled
- [x] final report behavior validated

Repository-native final-state evidence is the PBIP/TMDL/PBIR source plus the final Mermaid model diagram. A duplicate after-state PNG is not required as a release gate.

## Phase D — Independent no-tutorial audit ✅

- [x] model can be explained from business event → grain → facts/dimensions → relationships → measures → security
- [x] Append vs Merge vs separate-fact decisions can be justified without the tutorial
- [x] at least three architecture decisions and trade-offs documented
- [x] real modeling failures and fixes documented
- [x] role-playing Date/Geo logic can be explained independently
- [x] RLS filter path can be traced independently
- [x] project limitations and production caveats documented

See [`docs/12_final_audit.md`](docs/12_final_audit.md).

## Final validated references

- Order lines: **200**
- Distinct Orders: **80**
- Total Sales: **526,643.91**
- Active Customers: **47**
- Customers in `dim_customer`: **60**
- Total Target Revenue: **552,000.00**
- Target Attainment: **~95.4%**
- `fact_order_process`: **80 rows / 80 Orders**
- Orders with Payment: **60**
- Average Order → Pay: **~32.93 days**

Diagnostic failure evidence retained:

> Naive lifecycle-child merges produced **97 rows for 80 Orders** before grain-hardening.

## Definition of done — PASSED

The Data Modeling phase is complete because the model can now be explained and validated without step-by-step tutorial dependence:

- determine table and measure grain;
- distinguish facts and dimensions;
- design a star/galaxy schema;
- choose and justify cardinality/filter direction;
- identify many-to-many/ambiguity risks;
- recognize normal, junk and role-playing dimensions;
- choose Append vs Merge vs separate facts based on event/grain/shape;
- diagnose row multiplication after joins/merges;
- reconcile business metrics after model changes;
- trace and test an RLS security path;
- explain architecture decisions, limitations and failure modes.

## Closure

**Theory:** complete.  
**Guided implementation:** complete.  
**Runtime validation:** complete.  
**Independent audit:** complete.  
**Portfolio status:** finalized.