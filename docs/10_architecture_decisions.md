# 10 — Architecture and Modeling Decisions

This log captures the main capstone decisions synchronized with the current PBIP/TMDL model.

## ADR-001 — Separate staging, dimensions, facts and support queries

**Decision:** organize Power Query into `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`.  
**Reason:** clearer lineage and responsibility.  
**Trade-off:** more explicit project structure.  
**Validation:** query groups exist in the semantic model.

## ADR-002 — Append yearly Orders

**Decision:** `ORDERS_2025` + `ORDERS_2026` → `orders`.  
**Reason:** same event, same Order grain, compatible shape.  
**Rejected alternative:** duplicate yearly analytical models.  
**Validation:** `Table.Combine` is source-controlled.

## ADR-003 — Consolidate customer context

**Decision:** build one `dim_customer` from customer master/contact/user/address/city context.  
**Reason:** report-friendly Star-schema context rather than source fragmentation.  
**Control:** primary contacts are filtered before merge so customer grain is protected.

## ADR-004 — Flatten product/category context and add model key

**Decision:** `dim_product` combines product and category context and creates `product_key`.  
**Reason:** avoid unnecessary analytical snowflake; separate model key from source code.

## ADR-005 — Use a Junk Dimension for order flags

**Decision:** channel/status/priority combinations → `dim_order_flags`.  
**Reason:** avoid repetitive descriptors in Sales and several tiny dimensions.

## ADR-006 — Manual channel mapping only for the guided case

**Decision:** use local `channels` lookup.  
**Trade-off:** simple for the controlled project but creates ownership/maintenance risk.  
**Preferred production alternative:** authoritative upstream mapping.

## ADR-007 — Sales fact at Order-Line grain

**Decision:** build `fact_sales` from `order_line_items`; remove Order-level `OrderTotal`.  
**Reason:** additive Sales must follow the detail grain; repeated Order-level totals would double count.  
**Validation:** `total_sales` uses `line_total`; Orders use `DISTINCTCOUNT(order_id)`.

## ADR-008 — Shared Geo dimension with role-playing geography

**Decision:** reuse `dim_geo` for Ship-To and Bill-To; Ship-To is active and Bill-To inactive.  
**Reason:** one geography entity, multiple business roles, one clear default filter path.

## ADR-009 — Keep different business events as separate facts

**Decision:** Sales, Inventory, Campaign Spend, Promotion Coverage, Order Process and Sales Targets remain separate facts.  
**Reason:** they have different grains/events; direct fact coupling would create ambiguous or many-to-many semantics.  
**Validation:** relationships file contains no direct fact-to-fact relationship.

## ADR-010 — Harden the Order Process as one row per Order

**Context:** naive merges of Shipments/Invoices/Payments created recorded fan-out (97 rows for 80 Orders).  
**Decision:** aggregate lifecycle child records into milestone tables before joining to the Order spine.  
**Reason:** preserve the declared accumulating-snapshot grain.  
**Trade-off:** milestone semantics (earliest/latest) must be explicit.  
**Validation:** final M query groups child events before merge; runtime 80/80 confirmation remains a release check.

## ADR-011 — Shared Date dimension and role-playing process dates

**Decision:** use `dim_date` as the analytical calendar. `fact_order_process[order_date]` is active; Ship/Delivery/Invoice/Pay relationships are inactive alternatives.  
**Reason:** one clear default time path plus explicit alternative roles.  
**Caveat:** Power BI Auto Date/Time local tables remain serialized and are technical debt.

## ADR-012 — Dynamic regional RLS at Customer context

**Decision:** role `regional access` filters `dim_customer[region]` from the `security` mapping using `USERPRINCIPALNAME()`.  
**Reason:** user-dependent regional access without one static role per Region.  
**Validation boundary:** implementation is source-controlled; runtime `View As` tests remain open.

## ADR-013 — Preserve unmapped Product facts explicitly

**Context:** Business Overview exposed an unexplained blank Product Category.  
**Decision:** introduce `product_key = 0` / `Unmapped Product` and map null Product lookups to that key.  
**Reason:** preserve sales rows and surface source-quality exceptions instead of filtering them away.  
**Validation boundary:** latest Power BI refresh still required.

## ADR-014 — Keep the report intentionally simple

**Decision:** maintain `Business Overview` + `Model Validation`, using a small set of business KPIs/charts.  
**Reason:** this portfolio proves semantic-model quality, not dashboard-design specialization.  
**Trade-off:** visual design is intentionally secondary to grain, relationships, reconciliation and security.