# Confusion Log

Use this file to convert vague uncertainty into specific questions that can be resolved and tested.

| Date | Topic | Specific question | Working hypothesis | Resolution / evidence | Status |
|---|---|---|---|---|---|
| 2026-09-01 | Bidirectional filtering vs ambiguity | Is ambiguity simply the same as filtering in both directions? | If both tables can filter each other, that may be what ambiguity means. | Corrected: bidirectional filtering describes filter direction across a relationship. Ambiguity means multiple active filter paths exist between parts of the model. Bidirectional filtering can create additional paths and therefore contribute to ambiguity, but the concepts are different. | Resolved |
| 2026-09-01 | Inactive relationships | Does an inactive relationship mean a 1:1 split or a dimension that was snowflaked? | Inactive relationships might mainly occur between split dimensions and therefore not be needed for filtering. | Corrected: an inactive relationship is a semantically valid relationship that exists in the model but is not used automatically as the active filter path. It is not defined by cardinality and is not the same as snowflaking. | Resolved |
| 2026-09-01 | Role-playing date dimension | If `fact_sales` has `order_date` and `ship_date` but only one `dim_date`, is this many-to-many? | Multiple date columns may imply many-to-many relationships. | Corrected: `dim_date[date]` remains unique on the `1` side, while both fact date columns repeat on the `*` side. Two `1:*` relationships can exist; typically one is active and another inactive. | Resolved |
| 2026-09-01 | Role-playing relationship activation | Are alternative role relationships inactive because multiple active relationships are generally not allowed? | Multiple role relationships may simply be forbidden from being active at the same time. | Corrected: the important modeling reason is preserving an unambiguous default filter path. Alternative relationships can be semantically valid but inactive so that competing active paths do not create ambiguity. | Resolved |
| 2026-09-01 | Junk dimension terminology | Is the pattern called a `Chunk Dimension`? | The name may be `Chunk Dimension`. | Corrected terminology: the course pattern is `Junk Dimension`, used to bundle heterogeneous low-level flags / descriptive attributes that do not justify separate dimensions. | Resolved |

## Rules

1. Write a concrete question rather than `I do not understand this`.
2. State a hypothesis before looking up the answer when possible.
3. Resolve the question using the course, model behavior or a controlled test.
4. Add the result to the relevant documentation if it changes the mental model.
5. Re-test the concept later without notes.
