# Project Plan

## Goal

Build defensible evidence of dimensional data modeling competence, progressing from course fundamentals to a validated Power BI capstone and an independent model audit.

## Phase A — Theory foundations ✅ COMPLETE

All seven theory lessons are complete and were checked through Active Recall before the guided capstone began.

## Phase B — Guided Nightmare portfolio project ▶ CURRENT

**Current video checkpoint: 03:45:58.**

### B1 — Prepare and investigate

- [x] PBIP/TMDL project initialized
- [x] course workbook kept local-only and source repointed
- [x] successful refresh after source repoint
- [x] Power Query groups created: `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`
- [x] source tables explored at business/domain level
- [x] initial fact/dimension candidates identified
- [x] order header vs order-line detail grain distinguished
- [ ] before-state screenshot committed under `screenshots/before/`
- [ ] protected fact baseline metrics recorded before fact reshaping

### B2 — Build dimensions ▶ IN PROGRESS

- [x] `dim_customer` built from customer-related source tables
- [x] customer dummy/test row filtered and non-analytical technical columns removed
- [x] address/city/region context consolidated into `dim_customer`
- [x] `dim_product` built and enriched from `subcategories`
- [x] product data-quality rows handled as demonstrated in the guided project
- [x] model-generated `product_key` created
- [x] `ORDERS_2025` and `ORDERS_2026` appended into `orders`
- [x] `dim_order_flags` extracted as a junk dimension from channel/status/priority combinations
- [x] manual `channels` mapping created and merged
- [ ] final junk-dimension naming/polish step
- [ ] remaining guided dimensions

### B3 — Build facts and relationships ⬜ NEXT

At 03:45:58 the first fact has **not yet been created**. The next guided sequence starts after the junk-dimension polish and uses `order_line_items` as the detail-grain source for `fact_sales`.

Planned controls:

1. state the fact grain before transformation;
2. capture/protect the baseline sales number;
3. merge header/context only with row-count/total protection;
4. add model keys for dimensional relationships;
5. preserve measures at the correct semantic grain;
6. connect facts through dimensions, never directly fact-to-fact.

### B4 — Semantic layer and security ⬜

- Date / role-playing relationships
- semantic measures
- baseline reconciliation
- documented RLS filter path
- representative role/user testing

### B5 — Final validation ⬜

- before/after reconciliation
- final model diagram and screenshots
- relationship/security validation
- architecture decisions and trade-offs

## Confirmed implementation principles

The guided implementation now demonstrates these decisions in the actual PBIP/TMDL model:

- same event + same grain + compatible structure can be appended (`ORDERS_2025` + `ORDERS_2026` → `orders`);
- related descriptive source tables can be consolidated into consumer-friendly dimensions (`dim_customer`, `dim_product`);
- surrogate/model keys are introduced where the source does not provide the desired model key (`product_key`);
- low-cardinality miscellaneous order attributes can be grouped into a junk dimension (`dim_order_flags`);
- manual reference mappings introduce maintenance responsibility and should preferably be owned upstream in a production system.

## Phase C — Independent evidence

After the guided implementation:

- re-audit the model without the tutorial;
- justify at least three modeling decisions;
- deliberately introduce and diagnose one relationship/modeling failure;
- document root cause, fix and prevention;
- reproduce the target model structure from business requirements without step-by-step guidance.

## Definition of done

The Data Modeling phase is complete only when I can, without step-by-step guidance:

- determine table grain and distinguish row grain from measure grain;
- distinguish facts and dimensions;
- design a star/galaxy schema;
- choose and justify relationship cardinality and filter direction;
- identify many-to-many and ambiguity risks;
- recognize normal, junk and role-playing dimension patterns;
- choose Append vs Merge vs separate facts based on event, grain and shape;
- reconcile important metrics after model changes;
- trace and test an RLS security path;
- explain architecture trade-offs and limitations.

## Portfolio evidence required

- before/after model evidence;
- source assessment;
- grain matrix;
- dimension/fact documentation;
- relationship matrix;
- baseline and reconciliation tests;
- RLS evidence;
- architecture decisions;
- final validation checklist;
- lessons learned and independent audit.

## Current gate

**Theory gate: passed.**  
**Guided implementation: dimension-building phase in progress.**  
**Immediate next action:** finish `dim_order_flags` polish, then start `fact_sales` from `order_line_items` and protect the first baseline metric.