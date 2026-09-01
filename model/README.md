# Power BI Model Artifacts

This directory contains the guided Nightmare Data Modeling capstone implementation.

## Current status

**Theory phase: complete.**  
**Nightmare Power BI implementation: initialized.**  
**Current implementation phase: source-model audit before redesign.**

The Power BI Project has now been committed in Git-friendly PBIP/TMDL form. No dimensional redesign is claimed yet: the committed model represents the imported Nightmare starting state that must first be inspected, documented and baselined.

## Current project structure

```text
model/
├── README.md
└── nightmare-data-model/
    ├── nightmare-data-model.pbip
    ├── nightmare-data-model.Report/
    └── nightmare-data-model.SemanticModel/
```

The semantic model is stored in **TMDL** under the `definition/` directory, which allows table and relationship changes to be reviewed as text-based Git diffs rather than only as a monolithic binary file.

## Source data policy

The guided project uses Data with Baraa's Nightmare source workbook, `dataset.xlsx`.

The workbook is **not committed to this portfolio repository**. Keep a local copy at:

```text
local-data/dataset.xlsx
```

See [`../local-data/README.md`](../local-data/README.md) for setup instructions.

Reasons:

- the workbook originates from third-party course material;
- this repository should demonstrate the modeling work rather than redistribute course assets;
- the PBIP/TMDL project already provides inspectable implementation evidence without committing the raw workbook;
- large/binary source files add poor Git history compared with text-based model definitions.

The completed Data with Baraa solution file must also remain outside this repository and should be used only as a later reference/check, not as implementation evidence.

## Power BI source-control rules

Tracked:

- `.pbip`
- `.pbir`
- `.pbism`
- `.tmdl`
- `.platform`
- report `definition/`
- semantic-model `definition/`
- `.pbi/editorSettings.json`

Ignored:

- `.pbi/localSettings.json`
- `.pbi/cache.abf`
- `.pbi/unappliedChanges.json` in this repository
- `.pbix` / `.pbit` binary artifacts
- the local `dataset.xlsx`

`editorSettings.json` remains tracked because it is semantic-model editor metadata intended to be shared across users/environments. `unappliedChanges.json` is deliberately ignored here so a portfolio commit represents applied Power Query/model state rather than unfinished local query changes.

## Current portability limitation

The imported source queries currently reference the Excel workbook through a local `File.Contents(...)` path. This is normal for a file-based Power BI source but means a different machine must repoint the source to its own local `dataset.xlsx` before refresh.

For the local development copy, use:

```text
<repository-root>/local-data/dataset.xlsx
```

After changing the source path in Power BI Desktop, refresh once and save the PBIP before beginning model transformations.

## Capstone evidence workflow

```text
Import Nightmare source
→ capture starting state
→ inventory source tables
→ establish baseline metrics
→ define grain
→ build dimensions
→ build facts
→ establish relationships
→ validate filter behavior
→ implement security
→ reconcile metrics
→ document decisions
→ independent audit
```

## Recommended milestone commits

```text
feat: initialize nightmare PBIP source model

docs: audit nightmare source tables and grain

test: record protected baseline metrics

feat: build and validate dimensions

feat: define facts and dimensional relationships

feat: implement role-playing date relationships

feat: add semantic measures

feat: implement row-level security

test: reconcile final model metrics
```

## Artifact rules

- do not commit temporary Power BI cache/local-setting files;
- do not commit credentials or secrets;
- do not commit the source course workbook unless redistribution permission for that exact asset is explicitly verified;
- do not commit the instructor's completed solution as if it were project evidence;
- project documentation must match the committed model state;
- prefer meaningful PBIP/TMDL milestone commits over binary `final`, `final2`, `final-new` copies.

## Next evidence to create

- before-state model screenshot;
- source table inventory;
- initial grain hypotheses;
- relationship/cardinality/filter-direction risk assessment;
- protected baseline metrics.
