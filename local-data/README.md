# Local source data

This directory is the **local-only data landing zone** for the Nightmare Data Modeling capstone.

## Expected file

```text
local-data/
├── README.md      # tracked
└── dataset.xlsx   # local only, ignored by Git
```

`dataset.xlsx` is the source workbook used by the guided Data with Baraa Nightmare Data Modeling project.

## Why the workbook is not committed

The workbook originates from third-party course material. This portfolio documents and versions the modeling work, not a redistribution of the original course dataset. The source workbook therefore remains local unless explicit redistribution terms for this specific Power BI course dataset are verified.

The repository instead versions:

- PBIP project files;
- TMDL semantic-model definitions;
- Power Query / model logic contained in the PBIP project;
- original documentation and diagrams;
- model screenshots created during the implementation;
- validation and reconciliation evidence.

## Local setup

1. Obtain `dataset.xlsx` from the original Data with Baraa course materials.
2. Copy it locally to:

   ```text
   local-data/dataset.xlsx
   ```

3. In Power BI Desktop, update the workbook source to this local file if the current query source points to another machine-specific path.
4. Refresh the model and verify that all expected source tables load before continuing the source-model audit.

## Portability note

Power Query `File.Contents(...)` can store a machine-specific absolute file path. The committed PBIP/TMDL files may therefore show the current developer's local source path. The workbook itself is intentionally not versioned. Before running the project on another machine, update the data-source path to that machine's local copy of `dataset.xlsx`.

## Do not add here

- the completed course solution PBIX/PBIP;
- credentials or secrets;
- private production data;
- exports that are not required as evidence.
