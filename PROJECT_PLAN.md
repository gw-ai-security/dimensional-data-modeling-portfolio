# Project Plan

## Goal

Build defensible evidence of dimensional data modeling competence: theory → guided implementation → validation/debugging → independent reproduction.

## Phase A — Theory foundations ✅ COMPLETE

All seven theory lessons were completed and checked through Active Recall.

## Phase B — Guided Nightmare implementation ✅ SOURCE-CONTROLLED IMPLEMENTATION COMPLETE

### B1 — Prepare and investigate ✅

- [x] PBIP/TMDL project initialized
- [x] course workbook kept local-only and source repointed
- [x] Power Query groups created: `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`
- [x] source/business inventory documented
- [x] starting Model View committed at `screenshots/before/before.png`
- [x] header/detail and major source grains documented

### B2 — Dimensions ✅

- [x] `dim_customer`
- [x] `dim_product`
- [x] `dim_order_flags`
- [x] `dim_geo`
- [x] `dim_campaign`
- [x] `dim_date`
- [x] final junk-dimension naming standardized
- [x] manual channel mapping risk documented
- [x] explicit Unmapped Product member introduced in the latest source-controlled design

### B3 — Facts ✅

- [x] `fact_sales` — Order-Line grain
- [x] `fact_inventory` — Product × Month grain
- [x] `fact_campaign_spend` — Campaign × Date event grain
- [x] `fact_promotion_coverage` — Campaign × Product grain
- [x] `fact_order_process` — one row per Order
- [x] `fact_sales_targets` — Period grain
- [x] order-process merge fan-out diagnosed and the query hardened by milestone aggregation

### B4 — Relationships / semantic layer ✅ IMPLEMENTED

- [x] dimensions connected to facts; no direct fact-to-fact relationship
- [x] shared Date relationships defined
- [x] role-playing Order Process dates use active/inactive relationships
- [x] Ship-To/Bill-To geography roles represented with active/inactive relationships
- [x] `_measures` table contains core business measures
- [x] `regional access` dynamic RLS role exists in TMDL
- [x] `Business Overview` and `Model Validation` pages exist in PBIR

## Phase C — Final validation and independent evidence ▶ CURRENT

Source-controlled implementation is not the same as release validation. Remaining gates:

- [ ] Power BI Desktop refresh succeeds on the current `main` state after the latest Unmapped Product changes
- [ ] Sales Trend and Sales-vs-Target render correctly after Date normalization
- [ ] Unmapped Product appears explicitly rather than as unexplained `(Blank)`
- [ ] `fact_order_process` confirms 80 rows / 80 distinct orders at runtime
- [ ] dynamic RLS tested with at least two representative users using `View As`
- [ ] final model screenshot committed under `screenshots/after/`
- [ ] final Business Overview screenshot committed under `screenshots/after/`
- [ ] independent no-tutorial model audit completed
- [ ] model explained from memory without tutorial dependence

## Recorded QA evidence

The local audit recorded:

- 200 order lines
- 80 distinct orders
- Total Sales = 526,643.91
- 47 active customers
- 60 dimension customers
- Target Revenue = 552,000.00
- Target Attainment ≈ 95.4%
- naive order-process merge = 97 rows for 80 orders
- hardened order-process design = intended 80 rows / 80 orders
- 60 orders with payment
- Average Order → Pay ≈ 32.93 days

See `tests/` for evidence provenance and open runtime gates.

## Definition of done

The Data Modeling phase is complete only when I can, without step-by-step guidance:

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

## Current gate

**Theory:** passed.  
**Guided implementation:** built and source-controlled.  
**Current focus:** Power BI runtime validation + independent no-tutorial audit (Issues #9 and #10).