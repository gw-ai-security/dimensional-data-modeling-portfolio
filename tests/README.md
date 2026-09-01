# Validation Evidence

This directory will hold model validation and reconciliation evidence for the Nightmare capstone.

The goal is to prove that structural changes preserve or intentionally change business results rather than relying on visual inspection alone.

## Current status

**Theory phase complete; capstone test results not started.**

Templates do not count as passed tests. A test becomes evidence only after it is executed against the actual Power BI model and the result is recorded.

## Validation categories

1. **Baseline metrics** — known totals before remodeling.
2. **Grain validation** — explicit table grain and important measure/column grain.
3. **Dimension key validation** — uniqueness on intended `1` sides.
4. **Relationship validation** — cardinality, filter direction, active/inactive state and ambiguity checks.
5. **Multiple-fact reconciliation** — standalone fact totals vs combined shared-dimension visuals.
6. **Semantic-measure validation** — aggregations respect measure grain.
7. **RLS validation** — representative users/roles, allowed/forbidden scope and restricted totals.
8. **Final validation** — release-style checklist and before/after reconciliation.

## Minimum workflow

```text
Capture baseline
→ change model
→ test structural assumptions
→ reconcile measures
→ test security if applicable
→ document result
```

## Critical failure patterns to test for

- duplicate dimension keys changing intended `1:*` relationships;
- accidental many-to-many relationships;
- bidirectional filters creating unexpected paths;
- ambiguous active paths;
- higher-grain measures double counted at lower-grain rows;
- fan-out when combining different-grain facts;
- multi-fact visuals that change standalone totals;
- security filters that do not propagate to protected facts;
- RLS user mappings that expose unauthorized rows.

## Definition of valid evidence

Each important test should record:

- what was tested;
- expected result;
- actual result;
- pass/fail;
- evidence reference (model screenshot, metric table, query/check, or documented Power BI test);
- remediation if failed.

No capstone test is marked passed before implementation begins.