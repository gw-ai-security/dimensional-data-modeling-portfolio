# Dimensional Data Modeling Portfolio

> Evidence-driven Power BI dimensional-modeling portfolio: a deliberately messy analytical source model is assessed, reshaped into a star/galaxy semantic model, validated against business metrics, secured with Dynamic RLS, and exposed through a focused business report.

## Status

**FINALIZED — guided implementation, Power BI runtime validation and independent no-tutorial audit complete.**

Completed scope:

- ✅ Lessons 1–7 completed through Active Recall
- ✅ Nightmare source model assessed and before-state captured
- ✅ Power Query organized into `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`
- ✅ six analytical dimensions implemented
- ✅ six fact tables implemented at explicit grains
- ✅ shared and role-playing relationships validated
- ✅ semantic measure table implemented and reconciled
- ✅ Dynamic regional RLS implemented and tested with representative `View As` scenarios
- ✅ `Business Overview` and `Model Validation` report pages validated
- ✅ fan-out, date-key and unmapped-product defects diagnosed and corrected
- ✅ independent no-tutorial audit completed

The PBIP/TMDL files are the implementation source of truth. Final closure evidence is documented in [`docs/12_final_audit.md`](docs/12_final_audit.md).

## Problem

The course case starts from a deliberately chaotic analytical model: fragmented customer/product context, yearly source splits, mixed grains, weak relationship topology, wide inventory data, multiple business events and security requirements.

The modeling objective is not a prettier diagram. It is a reliable semantic layer:

```text
Understand business events
→ state grain
→ build dimensions
→ build facts
→ protect row counts and totals
→ establish dimensional relationships
→ add semantic measures
→ apply/test RLS
→ reconcile
→ prove the model through reporting
```

## Final semantic model

### Dimensions

- `dim_customer` — one row per customer
- `dim_product` — one row per analytical product/member, including an explicit unmapped member
- `dim_order_flags` — one row per unique channel/status/priority combination
- `dim_geo` — geographic lookup reused for Ship-To/Bill-To roles
- `dim_campaign` — one row per campaign
- `dim_date` — shared calendar dimension

### Facts

- `fact_sales` — one row per order line
- `fact_inventory` — one row per product-month inventory snapshot
- `fact_campaign_spend` — one row per campaign-date activity record
- `fact_promotion_coverage` — one row per campaign-product combination
- `fact_order_process` — one row per order lifecycle
- `fact_sales_targets` — one row per target period

Facts are analyzed through shared dimensions; there is no direct fact-to-fact relationship in the final semantic model. `fact_order_process` uses active/inactive Date relationships for role-playing process dates. `fact_sales` uses active Ship-To geography and an inactive Bill-To alternative.

See [`diagrams/capstone-progress.md`](diagrams/capstone-progress.md) and [`tests/relationship_validation.md`](tests/relationship_validation.md).

## Engineering QA findings

### 1. Order-process merge fan-out

A naive merge of one-to-many lifecycle child events produced **97 rows for 80 distinct Orders**, violating the intended one-row-per-Order grain.

The final design aggregates lifecycle milestones before joining them to the Order spine:

```text
shipments → Order milestones
payments  → Invoice milestone
invoices  → Order milestones
                ↓
orders → fact_order_process
         1 row = 1 order
```

Final runtime validation confirmed the intended **80 rows / 80 distinct Orders**.

### 2. Date-key compatibility

The first report smoke test showed correct headline Sales but a blank Sales Trend. The issue was semantic, not visual: `fact_sales[order_date]` needed normalization to Date grain for the shared `dim_date` relationship. The corrected path was revalidated in Power BI.

### 3. Unmapped Product handling

An unexplained `(Blank)` Product Category exposed a referential-integrity exception. The final design preserves those fact rows and maps unmatched products to an explicit `product_key = 0` / `Unmapped Product` member instead of hiding or dropping them.

## Validated business references

| Metric | Final reference |
|---|---:|
| Order lines | 200 |
| Distinct Orders | 80 |
| Total Sales | 526,643.91 |
| Active Customers | 47 |
| Customers in `dim_customer` | 60 |
| Total Target Revenue | 552,000.00 |
| Target Attainment | ~95.4% |
| `fact_order_process` rows | 80 |
| Orders with Payment | 60 |
| Average Order → Pay | ~32.93 days |

See [`tests/baseline_metrics.md`](tests/baseline_metrics.md), [`tests/reconciliation_tests.md`](tests/reconciliation_tests.md) and [`tests/final_validation.md`](tests/final_validation.md).

## Semantic measures

The `_measures` table defines:

- `total_sales`
- `total_orders`
- `total_active_customers`
- `base_total_customers`
- `avg_order_to_pay`
- `total_target_revenue`
- `target_attainment_pct`

Definitions and grain rationale are documented in [`docs/08_semantic_measures.md`](docs/08_semantic_measures.md).

## Dynamic RLS

The model contains a `regional access` role using `USERPRINCIPALNAME()` plus the `security` mapping table to restrict `dim_customer[region]`. Representative `View As` scenarios were executed and the expected Region-scoped customer/sales behavior was confirmed.

The project intentionally does not commit real user identities or confidential access mappings. See [`docs/09_security.md`](docs/09_security.md).

## Reporting proof

The PBIP report contains two pages:

- **Business Overview** — focused business-facing proof of the semantic model
- **Model Validation** — model/measure validation surface retained from the implementation workflow

The Business Overview is intentionally compact. This portfolio demonstrates Data Modeling / semantic-layer quality rather than dashboard-design specialization.

## Repository structure

```text
.
├── README.md
├── PROJECT_PLAN.md
├── SOURCES.md
├── docs/
├── diagrams/
├── learning/
├── local-data/          # dataset.xlsx local-only
├── model/
│   └── nightmare-data-model/
├── screenshots/
└── tests/
```

## Final portfolio claim

> I completed and validated a guided redesign of a complex analytical Power BI model into a dimensional star/galaxy semantic model. I defined grains, built dimensions and facts, implemented shared and role-playing relationships, created grain-aware measures, added Dynamic RLS, diagnosed and fixed modeling defects, reconciled business metrics, and validated the model through a simple business report and an independent no-tutorial audit.

This repository does **not** claim production Power BI administration, enterprise-scale performance testing, deployment pipelines or production SLAs.

## Attribution

The course structure, case study and source dataset originate from **Data with Baraa**. The repository adds original documentation, Mermaid diagrams, Active Recall records, PBIP/TMDL implementation, QA findings, validation evidence and the independent final audit. See [`SOURCES.md`](SOURCES.md).

## License

Code and original documentation in this repository are covered by the repository license. Third-party course material and datasets remain subject to their original authors' terms.