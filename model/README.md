# Power BI Model Artifacts

This directory contains the guided Nightmare Data Modeling capstone in Power BI Project (PBIP) format.

## Current status

**Guided semantic-model implementation complete and source-controlled. Final runtime validation remains open.**

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

## Key modeling characteristics

- Order-Line Sales fact
- Product-Month Inventory fact
- Campaign Spend and Campaign-Product coverage facts
- Order-level accumulating process fact
- period Sales Target fact
- shared Customer/Product/Campaign/Geo/Date context
- active/inactive role-playing Date and Geo relationships
- dynamic regional RLS role
- centralized measures

## Source data policy

`dataset.xlsx` stays local at:

```text
local-data/dataset.xlsx
```

The instructor solution is not committed.

## Portability

Source expressions currently serialize an absolute local `File.Contents(...)` path. A different machine must repoint the workbook before refresh. This is a known local-development limitation, not a production ingestion design.

## Current technical caveats

- Power BI Auto Date/Time `LocalDateTable_*` artifacts remain serialized alongside explicit `dim_date`.
- the latest Unmapped Product change requires a clean Power BI refresh to prove the current source state;
- runtime RLS evidence still needs `View As` tests;
- final after-state screenshots are not yet committed.

See `../tests/final_validation.md` for the release gate.