# Screenshot Evidence

Screenshots will be added as the Nightmare capstone is implemented.

## Current status

**Theory phase complete; no capstone screenshots claimed yet.**

Recommended structure:

```text
screenshots/
├── before/
├── dimensions/
├── facts/
├── relationships/
├── validation/
└── security/
```

## Required evidence moments

Capture screenshots only when they prove a meaningful modeling state or validation result, for example:

- original source-model / relationship state before changes;
- important duplicate-key or data-quality issue;
- completed dimension/fact model state;
- relationship cardinality/filter-direction evidence;
- role-playing active/inactive relationship state;
- shared-dimension multi-fact model;
- final star/galaxy schema;
- RLS/security filter-path configuration;
- representative RLS test result;
- final validation/reconciliation result.

## Rules

- capture evidence that proves a modeling decision or test result;
- prefer readable crops over full-screen clutter;
- do not create a screenshot for every Power BI click;
- do not include credentials, real personal emails or confidential data;
- name files by purpose rather than `Screenshot1.png`;
- accompany important screenshots with explanatory text in the relevant documentation;
- screenshots must match the committed Power BI model state.