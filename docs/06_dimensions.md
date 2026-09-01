# 06 — Dimension Design

> **Status: implementation evidence in progress. Current guided checkpoint: 03:45:58.**

This file documents dimensions that actually exist in the committed PBIP/TMDL model. It does not claim dimensions that have not yet been built.

## `dim_customer`

### Business purpose

Provide one report-friendly source of customer context instead of leaving customer attributes fragmented across source tables.

### Grain

> One row represents one customer.

### Source inputs

- `CUST_MASTER`
- `customer_contacts`
- `user_details`
- `Address`
- `cities`

### Key

- source/natural identifier: `CustomerID`
- exposed model column at this checkpoint: `customer_id`

### Implemented transformations

- remove dummy/test customer `CustomerID = 9999`;
- merge primary contact context from `customer_contacts`;
- merge credit/phone context from `user_details`;
- merge address context through `Address`;
- merge region context through `cities`;
- remove source-only/technical attributes such as `hash_key` and `source_id`;
- rename output attributes toward snake_case and report-friendly names.

Current output includes:

```text
customer_id
customer_name
segment
account_manager
payment_terms
contact_name
contact_email
credit_limit
phone
street
city
region
```

### Validation principle

Each enrichment merge must preserve the customer grain. The guided project uses relationship/cardinality checks and row-count checks to build confidence that a lookup-side table will not duplicate customers.

---

## `dim_product`

### Business purpose

Provide one product dimension containing product identity plus category context required for analysis.

### Grain

> One row represents one product.

### Source inputs

- `products`
- `subcategories`

### Keys

- source business key: `product_code`
- model-generated surrogate/model key: `product_key`

The product source does not provide the desired model surrogate ID, so an index is created and renamed to `product_key`.

### Implemented transformations

- filter dummy row `ZZZ-000`;
- clean/split the source subcategory mapping;
- merge category into the product dimension using subcategory;
- handle the guided project problem rows `ELE-901` and `HOM-902`;
- create `product_key` using an index;
- remove `ProductDescription`, `hash_key`, `source_id` from the analytical dimension;
- rename and reorder attributes.

Current output:

```text
product_key
product_code
product_name
brand
subcategory
category
price
supplier
```

### Design rationale

The course source is partially snowflaked: product description and category mapping are split across source tables. The guided analytical design merges the useful category context into a single consumer-friendly product dimension rather than retaining an unnecessary snowflake.

---

## `dim_order_flags` — Junk Dimension

### Business purpose

Extract small repetitive order descriptors from the order-header data so they do not remain as separate scattered flags in the analytical fact.

### Grain

> One row represents one unique combination of order channel, status and priority.

### Source inputs

- `orders`
- manually entered staging lookup `channels`

### Key

- model-generated `flag_key`

### Implemented transformations through 03:45:58

1. reference the unified `orders` staging query;
2. retain `OrderChannel`, `Status`, `Priority`;
3. remove duplicates so the dimension contains unique combinations;
4. create `flag_key` via index;
5. merge `channels` on channel code;
6. replace the cryptic channel code with a descriptive channel name.

At the current checkpoint, the final column-name polish shown immediately afterward in the video has **not yet been completed**. The committed TMDL still exposes names such as `channels.channel_name`, `Status` and `Priority`. That is intentional documentation of the exact current state rather than a premature claim.

### Manual mapping risk

`channels` is manually entered model data. This introduces an operational dependency: if the source system adds or changes channel codes, the local mapping can become stale and produce unmatched/null values.

For a production system, the preferred design would move this mapping responsibility upstream so the source/data-contract owner supplies the authoritative mapping and refresh stays automated.

---

## Dimension implementation status

| Dimension | Grain documented | Built in PBIP | Key strategy | Cleanup documented | Status |
|---|---|---|---|---|---|
| `dim_customer` | ✅ | ✅ | source customer ID | ✅ | implemented |
| `dim_product` | ✅ | ✅ | model `product_key` + source code | ✅ | implemented |
| `dim_order_flags` | ✅ | ✅ | model `flag_key` | ✅ | built; final naming polish next |
| remaining guided dimensions | ⬜ | ⬜ | TBD | ⬜ | pending |

## Current validation checklist

- [x] dimension grain stated for implemented dimensions
- [x] source inputs documented
- [x] key strategy documented
- [x] unnecessary/technical attributes actively removed
- [x] dummy/problem rows handled where the guided project demonstrates them
- [x] junk-dimension purpose documented
- [x] manual mapping maintenance risk documented
- [ ] final `dim_order_flags` naming polished
- [ ] all remaining project dimensions completed
- [ ] final dimension-to-fact relationships validated