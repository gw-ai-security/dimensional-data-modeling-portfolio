# 10 — Architecture and Modeling Decisions

> **Status: COMPLETE — decisions are synchronized with the finalized PBIP/TMDL model and runtime validation.**

## ADR-001 — Separate staging, dimensions, facts and support queries

**Decision:** organize Power Query into `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`.  
**Reason:** clearer lineage and responsibility.  
**Validation:** query groups exist and refresh successfully.

## ADR-002 — Append yearly Orders

**Decision:** `ORDERS_2025` + `ORDERS_2026` → `orders`.  
**Reason:** same event, same Order grain, compatible shape.  
**Rejected alternative:** duplicate yearly analytical models.

## ADR-003 — Consolidate customer context

**Decision:** build one `dim_customer` from customer master/contact/user/address/city context.  
**Reason:** report-friendly Star-schema context rather than source fragmentation.  
**Control:** primary contacts are filtered before merge so customer grain is protected.

## ADR-004 — Flatten product/category context and add a model key

**Decision:** `dim_product` combines product and category context and creates `product_key`.  
**Reason:** avoid an unnecessary analytical snowflake and separate model key from source code.

## ADR-005 — Use a Junk Dimension for order flags

**Decision:** channel/status/priority combinations → `dim_order_flags`.  
**Reason:** avoid repetitive descriptors in Sales and several tiny dimensions.

## ADR-006 — Manual channel mapping only for the controlled case

**Decision:** use local `channels` lookup.  
**Trade-off:** simple for the course scenario but creates ownership/maintenance risk.  
**Preferred production alternative:** authoritative upstream mapping.

## ADR-007 — Sales fact at Order-Line grain

**Decision:** build `fact_sales` from `order_line_items`; remove Order-level `OrderTotal`.  
**Reason:** additive Sales must follow detail grain; repeated Order totals would double count.  
**Validation:** `total_sales` uses `line_total`; Orders use `DISTINCTCOUNT(order_id)`.

## ADR-008 — Shared Geo dimension with role-playing geography

**Decision:** reuse `dim_geo` for Ship-To and Bill-To; Ship-To is active and Bill-To inactive.  
**Reason:** one geography entity, multiple business roles, one clear default filter path.

## ADR-009 — Keep different business events as separate facts

**Decision:** Sales, Inventory, Campaign Spend, Promotion Coverage, Order Process and Sales Targets remain separate facts.  
**Reason:** different events/grains should not be coupled directly.  
**Validation:** no direct fact-to-fact relationship exists in the final TMDL model.

## ADR-010 — Harden the Order Process to one row per Order

**Context:** naive Shipments/Invoices/Payments merges produced **97 rows for 80 Orders**.  
**Decision:** aggregate lifecycle child records into explicit milestones before joining to the Order spine.  
**Reason:** preserve the declared Order grain.  
**Validation:** final runtime result is **80 rows / 80 distinct Orders**.

## ADR-011 — Shared Date dimension and role-playing process dates

**Decision:** use `dim_date` as the analytical calendar. `fact_order_process[order_date]` is active; Ship/Delivery/Invoice/Pay relationships are inactive alternatives.  
**Reason:** one clear default time path plus explicit alternative roles.  
**Validation:** Sales Trend and multi-fact time behavior were revalidated after Date normalization.  
**Caveat:** Power BI Auto Date/Time local tables remain serialized as technical artifacts.

## ADR-012 — Dynamic regional RLS at Customer context

**Decision:** role `regional access` filters `dim_customer[region]` from the `security` mapping using `USERPRINCIPALNAME()`.  
**Reason:** user-dependent access without one static role per Region.  
**Validation:** representative `View As` scenarios passed after the final refresh.

## ADR-013 — Preserve unmapped Product facts explicitly

**Context:** Business Overview exposed an unexplained blank Product Category.  
**Decision:** introduce `product_key = 0` / `Unmapped Product` and map null Product lookups to that key.  
**Reason:** preserve Sales rows and surface source-quality exceptions instead of filtering them away.  
**Validation:** final refresh/report check confirmed the explicit Unmapped member.

## ADR-014 — Keep the report intentionally simple

**Decision:** maintain `Business Overview` + `Model Validation` using a focused set of KPIs/charts.  
**Reason:** this portfolio proves semantic-model quality, not dashboard-design specialization.

## Closure

All decisions above are accepted and represented in the finalized project. Production hardening beyond this portfolio scope—deployment pipelines, upstream mapping ownership, service administration and performance/SLA engineering—is intentionally excluded.