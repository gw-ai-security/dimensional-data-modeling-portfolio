# Power BI Model Artifacts

This directory is reserved for the guided Nightmare Data Modeling capstone.

## Current status

**Theory phase: complete.**  
**Nightmare Power BI implementation: not started yet.**

No model artifact is committed yet. This is intentional: the repository does not claim practical implementation before the corresponding Power BI work exists.

## Preferred project format

If the installed Power BI Desktop version supports **Power BI Project (`.pbip`)**, prefer that format because its project structure is more Git-friendly than a monolithic binary `.pbix` file.

Suggested structure:

```text
model/
└── nightmare-data-modeling/
    ├── nightmare-data-modeling.pbip
    ├── <report project files>
    └── <semantic model project files>
```

If the course workflow or local Power BI version requires `.pbix`, a meaningful checkpoint/final `.pbix` may be stored instead, subject to GitHub file-size limits and redistribution/licensing considerations.

## Capstone evidence workflow

```text
Build in Power BI
→ save model artifact locally in this repo
→ commit meaningful milestones
→ update docs/tests/diagrams from the real implementation
```

Recommended milestone commits:

```text
feat: capture initial nightmare model state
feat: build and validate dimensions
feat: define fact grains and measures
feat: establish dimensional relationships
feat: implement role-playing date relationships
feat: add semantic measures
feat: implement row-level security
test: reconcile final model metrics
```

## Artifact rules

- do not commit temporary Power BI lock/cache files;
- do not commit credentials or secrets;
- verify whether embedded course data may be redistributed before publishing a `.pbix` containing it;
- project documentation must match the committed model state;
- do not keep meaningless `final`, `final2`, `final-new` binary copies in Git history.

## Planned evidence

- Power BI project/model artifact;
- model version/reproducibility notes;
- before/final model screenshots;
- relationships and security evidence;
- baseline and reconciliation results;
- final independent audit.