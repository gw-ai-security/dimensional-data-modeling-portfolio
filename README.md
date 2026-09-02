# Dimensional Data Modeling Portfolio

> Evidence-driven Power BI dimensional-modeling portfolio: a deliberately messy analytical source model is assessed, reshaped into a star/galaxy semantic model, tested against protected business metrics, secured with dynamic RLS, and exposed through a deliberately simple report.

## Project status

**Guided implementation complete; final runtime validation and the independent no-tutorial audit are still open release gates.**

The repository now contains the complete PBIP/TMDL implementation:

- ✅ theory Lessons 1–7 completed through Active Recall
- ✅ Nightmare source model assessed and before-state captured
- ✅ Power Query organized into `01_Stage`, `02_Dimensions`, `03_Facts`, `04_Support`
- ✅ six analytical dimensions implemented
- ✅ six fact tables implemented at explicit business grains
- ✅ shared and role-playing relationships defined
- ✅ semantic measure table implemented
- ✅ dynamic regional RLS role source-controlled
- ✅ `Business Overview` and `Model Validation` report pages source-controlled
- ✅ major grain/fan-out and date-key defects identified and corrected during QA
- ▶ final Power BI refresh/smoke-test evidence after the latest source changes
- ▶ representative `View As` RLS runtime tests
- ▶ final after-state/report screenshots committed to Git
- ⬜ independent no-tutorial audit

The PBIP/TMDL files are the implementation source of truth. Documentation distinguishes **implemented in source control** from **runtime-validated in Power BI Desktop**.

## Problem

The course case starts from a deliberately chaotic analytical model: fragmented customer/product context, yearly source splits, mixed grains, weak relationship topology, wide inventory data, multiple business events and security requirements.

The modeling objective is not to make a prettier diagram. It is to make the semantic layer reliable:

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
- `dim_product` — one row per analytical product, including an explicit unmapped member
- `dim_order_flags` — one row per unique channel/status/priority combination
- `dim_geo` — geographic lookup for city/region context
- `dim_campaign` — one row per campaign
- `dim_date` — shared calendar dimension

### Facts

- `fact_sales` — one row per order line
- `fact_inventory` — one row per product-month inventory snapshot
- `fact_campaign_spend` — one row per campaign-date activity record
- `fact_promotion_coverage` — one row per campaign-product coverage combination
- `fact_order_process` — one row per order/process lifecycle
- `fact_sales_targets` — one row per target period

The current relationship model contains no direct fact-to-fact relationship. Facts are analyzed through shared dimensions. `fact_order_process` uses active/inactive date relationships for role-playing process dates; `fact_sales` uses active Ship-To geography and an inactive Bill-To alternative.

See [`diagrams/capstone-progress.md`](diagrams/capstone-progress.md) and [`tests/relationship_validation.md`](tests/relationship_validation.md).

## Important QA findings

### 1. Merge fan-out in the order process

The intended grain of `fact_order_process` is **one row per order**. A naive merge of one-to-many child events can multiply order rows. During the local audit, the naive path produced **97 rows for 80 distinct orders**.

The final query aggregates lifecycle milestones before the join:

```text
shipments → OrderID milestones
payments  → InvoiceID milestone
invoices  → OrderID milestones
                ↓
orders → fact_order_process
         1 row = 1 order
```

This is a stronger portfolio result than merely reproducing the tutorial: a technically valid merge was rejected because it violated grain.

### 2. Date key compatibility

The first Business Overview smoke test showed correct headline Sales but a blank Sales Trend. `fact_sales[order_date]` still contained a DateTime representation while the shared calendar operates at Date grain. The Power Query output was normalized to Date before the shared relationship is used.

### 3. Unmapped product handling

The first report exposed an unexplained `(Blank)` Product Category. The latest source-controlled model introduces `product_key = 0` / `Unmapped Product` rather than deleting fact rows or hiding the blank visual category. This preserves fact totals and makes the source-quality exception explicit. A final Power BI refresh is still required to prove the latest query state at runtime.

## Recorded validation values

The following values were recorded from the local course dataset / Power BI model during the 2026-09-02 audit. The source workbook is intentionally not committed, so GitHub cannot independently recalculate them.

| Metric | Recorded value |
|---|---:|
| Order lines | 200 |
| Distinct orders | 80 |
| Total Sales | 526,643.91 |
| Active customers | 47 |
| Customers in `dim_customer` | 60 |
| Sales target | 552,000.00 |
| Target attainment | 95.4% |
| Hardened `fact_order_process` rows | 80 |
| Orders with payment | 60 |
| Average Order → Pay | ~32.93 days |

See [`tests/baseline_metrics.md`](tests/baseline_metrics.md) and [`tests/reconciliation_tests.md`](tests/reconciliation_tests.md).

## Semantic measures

The `_measures` table currently defines:

- `total_sales`
- `total_orders`
- `total_active_customers`
- `base_total_customers`
- `avg_order_to_pay`
- `total_target_revenue`
- `target_attainment_pct`

The business definitions and grain dependencies are documented in [`docs/08_semantic_measures.md`](docs/08_semantic_measures.md).

## Dynamic RLS

The model contains a source-controlled `regional access` role using `USERPRINCIPALNAME()` and the `security` mapping table to restrict `dim_customer[region]`. The implementation exists in TMDL; representative Power BI `View As` tests are intentionally still an open validation gate and are not claimed as complete merely because the role exists.

See [`docs/09_security.md`](docs/09_security.md).

## Reporting proof

The PBIP report contains two pages:

- **Business Overview** — simple business-facing proof of the semantic model
- **Model Validation** — model/measure validation surface retained from the project workflow

The Business Overview is intentionally small. This portfolio targets Data Modeling / semantic-layer evidence, not dashboard-design specialization.

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

## Evidence and claim boundary

This repository supports the claim:

> I completed a guided redesign of a complex analytical Power BI model, documented grain and model decisions, created dimensional facts/dimensions, implemented shared/role-playing relationships, semantic measures and dynamic RLS, and performed targeted QA that found and corrected modeling defects.

It does **not** claim:

- production Power BI administration;
- enterprise-scale performance testing;
- production refresh/deployment SLAs;
- independently completed no-tutorial modeling until Issue #10 is closed.

## Attribution

The course structure, case study and source dataset originate from **Data with Baraa**. The repository adds original documentation, Mermaid diagrams, Active Recall records, PBIP/TMDL implementation, QA findings and validation evidence. See [`SOURCES.md`](SOURCES.md).

## License

Code and original documentation in this repository are covered by the repository license. Third-party course material and datasets remain subject to their original authors' terms.