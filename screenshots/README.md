# Screenshot Evidence

## Current status

The starting Nightmare model is committed:

```text
screenshots/before/before.png
```

This proves the source-model state before dimensional redesign.

## Still required for final release evidence

Create and commit locally after the current PBIP passes its final Power BI smoke test:

```text
screenshots/after/final-model.png
screenshots/after/business-overview.png
screenshots/after/rls-validation.png   # optional but preferred; synthetic identity only
```

The final Model View screenshot should show the complete star/galaxy structure and readable relationships. The Business Overview screenshot should show all critical visuals without errors or unexplained blanks.

## Rules

- screenshots prove meaningful model/test states, not individual clicks;
- prefer readable evidence over full-screen clutter;
- do not include credentials or private identities;
- use synthetic RLS identities in public evidence;
- screenshots must match the committed PBIP state;
- do not use screenshots to claim a test that was not executed.

Chat/uploaded images are not automatically repository files; binary evidence must be committed from the local working copy.