# Sources and Attribution

## Primary learning sources

### Data Modeling in Power BI Full Course for Beginners — From Zero to Hero

- Author / channel: Data with Baraa
- URL: https://www.youtube.com/watch?v=pQSMbRA3O6g
- Role: primary source for the seven-lesson theory/fundamentals path in `docs/theory/`.
- Documentation basis: the user-provided transcript; repository notes paraphrase and organize the source rather than reproduce it verbatim.
- Theory/project boundary: the pre-project theory ends after Security/RLS; the full-course transcript transitions into the Nightmare project at approximately `02:40:08`.
- Status: complete through Active Recall.

### Power BI Data Modeling Portfolio Project End-to-End — Nightmare Data Model

- Author / channel: Data with Baraa
- URL: https://www.youtube.com/watch?v=0A2k62YEbfI
- Role: guided hands-on capstone implementation.
- Documentation basis: the user-provided project transcript plus the PBIP/TMDL artifacts created while working through the project.
- Status: guided implementation completed and source-controlled; final runtime validation and independent no-tutorial audit remain separate evidence gates.

## Nightmare source assets

### `dataset.xlsx`

The workbook is part of the Data with Baraa project materials and is intentionally **not redistributed** from this public portfolio. A local copy belongs at:

```text
local-data/dataset.xlsx
```

The repository versions the resulting PBIP/TMDL model, original documentation, diagrams, screenshots and validation evidence instead.

### Instructor solution

The completed instructor Power BI solution is not committed and is not presented as portfolio evidence. It is a reference/check only.

## Source hierarchy

For course-specific claims, the supplied transcripts remain the primary source. Implementation claims are grounded in the committed PBIP/TMDL state.

```text
Course transcript
→ theory notes
→ Active Recall / corrections
→ guided Power BI implementation
→ QA / reconciliation
→ independent audit
```

## Original contribution in this repository

- structured paraphrased learning documentation;
- original Mermaid diagrams;
- Active Recall and misconception tracking;
- hands-on PBIP/TMDL implementation;
- explicit grain/fact/dimension documentation;
- validation and reconciliation records;
- architecture decisions and trade-off analysis;
- debugging evidence, including merge fan-out and date-key failures;
- a simple report used as semantic-model proof;
- independent audit/reflection when completed.

## Claim boundary

The guided model implementation now exists in source control. Final Power BI runtime checks and the independent no-tutorial gate remain explicitly open; the repository does not equate tutorial completion with independent production competence.

Third-party course material and datasets remain subject to their original authors' terms.