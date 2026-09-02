# Local source data

This directory is the local-only data landing zone for the Nightmare Data Modeling capstone.

## Expected file

```text
local-data/
├── README.md      # tracked
└── dataset.xlsx   # local only, ignored by Git
```

`dataset.xlsx` originates from the Data with Baraa course materials and is intentionally not redistributed from this public portfolio.

The repository instead versions:

- PBIP/PBIR project definitions;
- TMDL semantic-model definitions;
- Power Query/model logic;
- original documentation and diagrams;
- project screenshots;
- validation/reconciliation evidence.

## Local setup

1. Obtain `dataset.xlsx` from the original course materials.
2. Copy it to `local-data/dataset.xlsx`.
3. Open `model/nightmare-data-model/nightmare-data-model.pbip`.
4. If required, repoint the Excel source to the local workbook.
5. Refresh and run the checks in `tests/final_validation.md`.

## Portability note

Power Query currently serializes a machine-specific absolute `File.Contents(...)` path. A different development machine must repoint the workbook. This repository does not present the local Excel source as a production ingestion architecture.

## Do not add here

- the completed instructor solution;
- credentials/secrets;
- private production data;
- generated exports that are not required as evidence.