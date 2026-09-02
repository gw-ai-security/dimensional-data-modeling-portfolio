# Power BI Model Artifacts

This directory contains the finalized Nightmare Data Modeling capstone in Power BI Project (PBIP) format.

## Status

**COMPLETE — semantic model, report, runtime refresh and representative RLS validation passed.**

## Project structure

```text
model/
├── README.md
└── nightmare-data-model/
    ├── nightmare-data-model.pbip
    ├── nightmare-data-model.Report/
    └── nightmare-data-model.SemanticModel/
```

TMDL/PBIR makes Power Query, model, role and report definitions inspectable as Git diffs.

## Analytical model objects

```text
02_Dimensions
├── dim_customer
├── dim_product
├── dim_order_flags
├── dim_geo
├── dim_campaign
└── dim_date

03_Facts
├── fact_sales
├── fact_inventory
├── fact_campaign_spend
├── fact_promotion_coverage
├── fact_order_process
└── fact_sales_targets

04_Support / semantic
├── security
└── _measures
```

The staging/source expressions remain available for lineage and transformation logic.

## Report pages

- `Business Overview`
- `Model Validation`

Both pages were revalidated after the final model refresh.

## Key modeling characteristics

- Order-Line Sales fact
- Product-Month Inventory fact
- Campaign Spend and Campaign-Product coverage facts
- Order-level process fact hardened against child-event fan-out
- Period Sales Target fact
- shared Customer/Product/Campaign/Geo/Date context
- active/inactive role-playing Date and Geo relationships
- explicit Unmapped Product member
- Dynamic regional RLS
- centralized semantic measures

## Source data policy

`dataset.xlsx` stays local at:

```text
local-data/dataset.xlsx
```

The instructor solution is not committed.

## Portability

Source expressions serialize an absolute local `File.Contents(...)` path. A different machine must repoint the workbook before refresh. This is a local-development limitation, not a production ingestion design.

## Technical caveats

- Power BI Auto Date/Time `LocalDateTable_*` artifacts remain serialized alongside explicit `dim_date`.
- the manual `channels` mapping would preferably be sourced upstream in production.
- the project does not include deployment pipelines, Power BI Service administration or enterprise-scale performance/SLA evidence.

These caveats do not block the finalized portfolio scope. See `../tests/final_validation.md` and `../docs/12_final_audit.md`.