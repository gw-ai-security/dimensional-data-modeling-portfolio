# Sources and Attribution

## Primary learning sources

### Data Modeling in Power BI Full Course for Beginners — From Zero to Hero

- Author / channel: Data with Baraa
- URL: https://www.youtube.com/watch?v=pQSMbRA3O6g
- Role in this repository: primary source for the theory/fundamentals learning path and the complete pre-project theory documentation in `docs/theory/`.
- Documentation basis: the user-provided full-course transcript. The notes paraphrase and organize the source rather than reproduce it verbatim.
- Theory/project boundary used in this repository: the theory block runs through the RLS section; the transcript then transitions into the guided Nightmare project at approximately `02:40:08`.
- Current status: theory block watched and completed through Active Recall on 2026-09-01.

### Power BI Data Modeling Portfolio Project End-to-End (Nightmare Data Model)

- Author / channel: Data with Baraa
- URL: https://www.youtube.com/watch?v=0A2k62YEbfI
- Role in this repository: guided capstone implementation after the theory block.
- Project documentation basis: the user-provided Data Modeling Project transcript plus artifacts created during the hands-on Power BI implementation.
- Current status: the PBIP project has been initialized from the Nightmare source workbook; the project is now in the source-model audit phase before any redesign.

## Nightmare source assets

### `dataset.xlsx`

The source workbook is part of the Data with Baraa Nightmare project materials. The project transcript describes the dataset as a deliberately chaotic starting point that should be investigated before dimensions and facts are rebuilt. fileciteturn213file4

The workbook itself is **not redistributed from this portfolio repository**. It should be kept locally at:

```text
local-data/dataset.xlsx
```

The repository tracks the resulting PBIP/TMDL model definitions, original documentation, diagrams, screenshots and validation evidence instead.

### Instructor solution

The completed Power BI solution supplied with the course is treated only as a later reference/check. It is not committed to this repository and is not used as evidence of the portfolio implementation.

## Source hierarchy

For course-specific claims, this repository follows the supplied transcripts. General modeling knowledge is not silently substituted for the course's terminology, sequence or recommendations.

```text
Course transcript
→ theory notes
→ Active Recall / corrections
→ guided Power BI implementation
→ validation evidence
→ independent audit
```

## Attribution policy

The course structure, guided case study and source dataset originate from Data with Baraa. This repository does not present those materials as independently invented.

The repository's original contribution consists of:

- structured paraphrased learning documentation;
- original Mermaid reconstructions;
- Active Recall and misconception tracking;
- hands-on PBIP/TMDL implementation evidence;
- validation/reconciliation evidence;
- architecture decisions and trade-off analysis;
- independent model audit/reflection.

Third-party screenshots, slides or course materials should not be copied into this repository unless licensing/permission clearly allows it. Original diagrams and model screenshots created during the implementation are preferred.

## Claim boundary

The pre-project theory block is complete and the Nightmare PBIP project has been initialized. The source model has **not yet been redesigned or validated**.

Practical modeling claims will be added only after the corresponding Power BI model changes, tests and validation evidence exist.

Third-party course material and datasets remain subject to their original authors' terms.