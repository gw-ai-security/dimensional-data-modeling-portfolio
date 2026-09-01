# 10 — Architecture and Modeling Decisions

This log captures actual capstone decisions that require modeling judgment. It is synchronized with the committed PBIP/TMDL state.

## ADR-001 — Separate staging, dimensions, facts and support queries

**Status:** Accepted

**Context**  
The imported Nightmare project mixes source-oriented objects and analytical concerns. Power Query will be used extensively for shaping the model.

**Decision**  
Organize queries into `01_Stage`, `02_Dimensions`, `03_Facts` and `04_Support`.

**Trade-off**  
Adds explicit project structure, but makes lineage and object purpose easier to understand as the model grows.

**Validation**  
The query groups exist in the committed PBIP/TMDL model.

---

## ADR-002 — Append yearly order headers into one `orders` staging query

**Status:** Accepted

**Context**  
`ORDERS_2025` and `ORDERS_2026` are source-system splits of the same business event. Both use Order grain.

**Decision**  
Append them into one `orders` staging query and remove source-only legacy/note fields that do not earn an analytical role.

**Alternatives considered**  
Keep separate yearly tables and maintain duplicate modeling logic.

**Trade-off**  
The unified object is simpler for downstream modeling; the original source split remains traceable in the staging definitions/Git history rather than becoming two analytical tables.

**Validation**  
The committed `orders` query uses `Table.Combine({ORDERS_2025, ORDERS_2026})` and retains one-order row semantics.

---

## ADR-003 — Consolidate customer context into `dim_customer`

**Status:** Accepted

**Context**  
Customer context is fragmented across customer master, contacts, user details, address and geography tables.

**Decision**  
Create one consumer-friendly `dim_customer` using controlled left merges and remove technical/unnecessary source columns.

**Alternatives considered**  
Preserve the fragmented source shape as a snowflake.

**Trade-off**  
The analytical model becomes easier to filter and understand, while Power Query carries more transformation logic. Merge cardinality must be controlled so customer rows are not duplicated.

**Validation**  
`dim_customer` exists in TMDL and contains the merged customer, contact, phone, address, city and region attributes.

---

## ADR-004 — Flatten product/category context and add a model key

**Status:** Accepted

**Context**  
The product source uses `ProductCode` as a business identifier and category context is split into a separate subcategory mapping.

**Decision**  
Build `dim_product`, merge the required category context into it and create a model-generated `product_key`.

**Trade-off**  
This avoids an unnecessary analytical snowflake and gives the semantic model a stable model-side key. The generated index is a model artifact rather than a source-system identity.

**Validation**  
The committed `dim_product` query contains the category merge and index-to-`product_key` transformation.

---

## ADR-005 — Extract order flags into a junk dimension

**Status:** Accepted

**Context**  
Order channel, status and priority are small, repetitive descriptors embedded in order-header data and do not justify separate tiny dimensions.

**Decision**  
Create `dim_order_flags` at the grain of one unique channel/status/priority combination and assign a model-generated `flag_key`.

**Trade-off**  
Reduces repetitive descriptors in the later fact and avoids several tiny dimensions. Users must understand that `flag_key` identifies a combination, not a source business entity.

**Validation**  
`dim_order_flags` exists in the committed TMDL and uses `Table.Distinct` plus an index key.

---

## ADR-006 — Use a manual channel lookup only as guided-project support

**Status:** Accepted with production caveat

**Context**  
The source order channel is coded and the project needs a friendly descriptive value. The guided project creates the mapping manually in Power Query.

**Decision**  
Use the local `channels` lookup for the course implementation and explicitly document its ownership/maintenance risk.

**Preferred production alternative**  
Have the source/data-product owner provide the authoritative mapping so new channel codes arrive through the automated source contract.

**Trade-off**  
Manual data is fast and clear for this controlled project but can become stale. New source values can produce null/unmapped outputs if nobody updates the lookup.

**Validation**  
`channels` and its merge into `dim_order_flags` are present in the committed model. The project documentation records the operational caveat.

---

## Next decisions to capture

- header/detail treatment while creating `fact_sales`;
- protected baseline and merge-safety strategy;
- dimension-key lookup strategy inside the fact;
- final fact separation/shared dimensions;
- date/role-playing relationship design;
- RLS security filter path.