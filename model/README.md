# Power BI Model Artifacts

This directory contains the guided Nightmare Data Modeling capstone implementation.

## Current status

**Theory phase: complete.**  
**Nightmare Power BI implementation: in progress.**  
**Current guided checkpoint: 03:45:58.**

The committed PBIP/TMDL model has progressed beyond the raw Nightmare starting state. Power Query staging groups and the first analytical dimensions now exist, while the first final fact has not yet been created.

## Project structure

```text
model/
├── README.md
└── nightmare-data-model/
    ├── nightmare-data-model.pbip
    ├── nightmare-data-model.Report/
    └── nightmare-data-model.SemanticModel/
```

The semantic model is stored in TMDL under `definition/`, making model changes inspectable as Git diffs.

## Implemented model objects through 03:45:58

### Query groups

```text
01_Stage
02_Dimensions
03_Facts
04_Support
```

### New/reshaped project objects

```text
01_Stage
├── orders          # append of ORDERS_2025 + ORDERS_2026
├── channels        # manually entered channel-code lookup
└── source/reference expressions used by dimensions

02_Dimensions
├── dim_customer
├── dim_product
└── dim_order_flags

03_Facts
└── no completed analytical fact yet at this checkpoint
```

`dim_order_flags` has been structurally built and enriched with `channels`; the final naming/polish step shown immediately after 03:45:58 in the course is still pending in the committed state.

## Source data policy

The guided project uses Data with Baraa's `dataset.xlsx`. The workbook is not committed. Keep the local copy at:

```text
local-data/dataset.xlsx
```

The completed instructor solution also remains outside this repository and is only a later reference/check.

Tracked PBIP source artifacts include `.pbip`, `.pbir`, `.pbism`, `.tmdl`, `.platform`, report/model `definition/` and shared editor metadata. Local cache/settings, binary PBIX/PBIT files, `unappliedChanges.json` and the course workbook remain ignored.

## Portability note

The source queries use a local `File.Contents(...)` path. A different development machine must repoint the workbook path before refresh. The local development copy currently targets the ignored repository-local workbook under `local-data/`.

## Evidence workflow — current position

```text
✅ Import Nightmare source
✅ Capture/inspect starting model in working session
✅ Organize Power Query workspace
✅ Explore source/business objects
✅ Build dim_customer
✅ Build dim_product
✅ Append yearly order headers → orders
✅ Extract dim_order_flags
✅ Add channels mapping
▶ Polish dim_order_flags
→ Reference order_line_items → fact_sales
→ Record/protect baseline sales metric
→ Merge header context safely
→ Add dimension keys
→ Build remaining facts/relationships
→ Date + measures + RLS
→ Reconcile and validate
```

## Current implementation rules

- state grain before combining tables;
- use Append only when event/grain/shape are compatible;
- check merge cardinality so rows are not multiplied;
- remove source attributes that do not earn a reporting role;
- keep one authoritative analytical home for descriptive context;
- document manually maintained mappings as an operational dependency;
- do not claim a final star schema until facts, dimensional relationships and reconciliation evidence exist.

## Next evidence to create

1. finish the junk-dimension naming polish;
2. create `fact_sales` from `order_line_items`;
3. record the protected sales baseline before/after risky merges;
4. commit the before-state Model View image to `screenshots/before/`;
5. continue documenting fact grain and model-key lookups.