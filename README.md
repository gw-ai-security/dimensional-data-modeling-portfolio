# Dimensional Data Modeling Portfolio

> Evidence-driven learning portfolio for dimensional data modeling: from modeling fundamentals and relationship semantics to the redesign and validation of a complex analytical model.

## Project status

**Theory complete — guided Nightmare implementation in progress. Current video checkpoint: 03:45:58.**

Current progress:

- ✅ Lessons 1–7 — complete and validated through Active Recall
- ✅ Nightmare PBIP/TMDL project initialized
- ✅ local course workbook repointed and refreshed
- ✅ Power Query workspace groups created: `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`
- ✅ source tables explored at business/domain level
- ✅ `dim_customer` built from customer-related source tables
- ✅ `dim_product` built and enriched with category context
- ✅ `ORDERS_2025` + `ORDERS_2026` consolidated into staging query `orders`
- ✅ junk-dimension structure `dim_order_flags` created
- ✅ manual channel-code mapping query `channels` created and joined into `dim_order_flags`
- ▶ next guided step: polish the junk dimension, then begin `fact_sales` from `order_line_items`
- ⬜ protected fact baseline metrics and first fact implementation
- ⬜ remaining facts, relationships, date model, measures, RLS and final reconciliation
- ⬜ independent no-tutorial model audit

The committed TMDL is the source of truth for implementation claims. At this checkpoint no completed fact table or final star/galaxy schema is claimed.

## Objective

This repository documents dimensional data modeling skills for analytics and data engineering, with emphasis on the semantic and structural layer underneath reporting:

- business-oriented table design
- fact and dimension modeling
- grain definition
- keys and cardinality
- relationship design and filter propagation
- star, snowflake and galaxy schemas
- special dimension patterns
- multiple-fact modeling
- data quality and reconciliation
- semantic measures
- row-level security
- modeling decisions and trade-offs

## Evidence model

```text
Concept → Explain → Implement → Validate → Debug → Document → Evidence
```

The repository separates three evidence levels:

1. **Course-derived concepts** — paraphrased in original documentation.
2. **Guided implementation** — the Data with Baraa Nightmare case reproduced hands-on.
3. **Independent evidence** — later validation, model audit, trade-off analysis and no-tutorial reconstruction.

## Theory curriculum — completed

The complete pre-project theory block is documented in [`docs/theory/`](docs/theory/README.md). Repository-native visual summaries are in [`diagrams/`](diagrams/README.md).

| Lesson | Topic | Status |
|---|---|---|
| 01 | Modeling foundations; facts and dimensions | ✅ |
| 02 | Star, snowflake and galaxy schemas | ✅ |
| 03 | Relationships, cardinality, filtering and ambiguity | ✅ |
| 04 | Extracted, junk and role-playing dimensions | ✅ |
| 05 | Grain and grain-aware aggregation | ✅ |
| 06 | Multiple facts, Append/Merge/shared dimensions | ✅ |
| 07 | Security and RLS | ✅ |

## Current implementation artifact

```text
model/nightmare-data-model/
├── nightmare-data-model.pbip
├── nightmare-data-model.Report/
└── nightmare-data-model.SemanticModel/
```

The semantic model is stored in TMDL, so Power Query, tables and relationships can be inspected as text-based Git diffs.

The course source workbook `dataset.xlsx` is not committed. A local copy belongs at `local-data/dataset.xlsx`; see [`local-data/README.md`](local-data/README.md). The instructor's completed solution stays outside the repository and is used only as a later reference/check.

## Implemented capstone work through 03:45:58

### Power Query organization

The project now uses explicit work areas:

```text
01_Stage
02_Dimensions
03_Facts
04_Support
```

This separates source-aligned staging/reference queries from analytical dimensions and later facts.

### Customer dimension

`dim_customer` is built from `CUST_MASTER` and enriched through left merges with customer contacts, user details, address and city/region context. Dummy/test data and technical source columns are removed, and output columns are renamed toward the project snake_case standard.

### Product dimension

`dim_product` is built from `products`, enriched from the cleaned `subcategories` mapping, filters known dummy/problem rows, creates a model-generated `product_key`, removes technical/non-analytical columns and exposes report-friendly attributes.

### Order staging and junk dimension

`ORDERS_2025` and `ORDERS_2026` represent the same order-header event at the same grain and are appended into `orders`. Source-only columns such as legacy/reference notes are removed.

From `orders`, the repeating low-cardinality descriptors `OrderChannel`, `Status` and `Priority` are extracted into `dim_order_flags`. A small manually entered `channels` lookup maps channel codes to descriptive names. At the 03:45:58 checkpoint the final polish/rename of this junk dimension is the next immediate guided step.

The manual mapping is intentionally documented as a maintenance risk: if source codes change, the mapping must be updated; a production-grade preference would be to source this mapping upstream rather than maintain it manually inside the model.

## Grain checkpoint

Confirmed working grains relevant to the current implementation:

```text
ORDERS_2025      → one row = one order
ORDERS_2026      → one row = one order
orders           → one row = one order
order_line_items → one row = one order line

dim_customer     → one row = one customer
dim_product      → one row = one product
dim_order_flags  → one row = one distinct channel/status/priority combination
```

The Order Header / Order Detail pattern is now explicit: `orders` is the header-level staging object, while `order_line_items` is the finer-grain detail source. The first sales fact is expected to use the detail grain, but it has not yet been created at this checkpoint.

## Current capstone path

```text
✅ initialize PBIP/TMDL
✅ repoint + refresh local source
✅ explore business/source tables
✅ organize Power Query groups
✅ build dim_customer
✅ build dim_product
✅ append order headers into orders
✅ extract dim_order_flags + channels mapping
▶ polish junk dimension
→ create fact_sales from order_line_items
→ record/protect fact baseline metrics
→ add dimension keys
→ build remaining facts
→ establish dimensional relationships
→ date model + measures
→ RLS
→ reconciliation + final validation
→ independent no-tutorial audit
```

See [`PROJECT_PLAN.md`](PROJECT_PLAN.md), [`docs/04_source_model_assessment.md`](docs/04_source_model_assessment.md), [`docs/05_grain_analysis.md`](docs/05_grain_analysis.md), [`docs/06_dimensions.md`](docs/06_dimensions.md) and the open implementation issues.

## Claim boundaries

The theory phase is complete and several guided dimensions/staging transformations are implemented in PBIP/TMDL. The final analytical model, facts, measures, security and reconciliation are not complete yet. This repository therefore demonstrates **guided implementation in progress**, not completed production Power BI experience.

## Attribution

The course structure, guided case study and source dataset are based on **Data with Baraa**. See [`SOURCES.md`](SOURCES.md). Course material is not presented as independently invented; this repository adds original documentation, model artifacts, validation evidence and later independent reconstruction.

## License

Code and original documentation in this repository are covered by the repository license. Third-party course material and source datasets remain subject to their original authors' terms.