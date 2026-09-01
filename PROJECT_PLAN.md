# Project Plan

## Goal

Build defensible evidence of dimensional data modeling competence, progressing from course fundamentals to a validated Power BI capstone and an independent model audit.

## Phase A — Foundations

| Module | Evidence | Status |
|---|---|---|
| Why data modeling | `docs/01_modeling_fundamentals.md` | ✅ documented |
| Facts vs dimensions | `docs/01_modeling_fundamentals.md` | ✅ documented |
| Star / Snowflake / Galaxy | `docs/02_schema_patterns.md` | ✅ documented |
| Relationships | `docs/03_relationships.md` | 🟡 learning in progress |
| Grain | `docs/05_grain_analysis.md` | ⬜ planned |
| Advanced modeling patterns | documentation + diagrams | ⬜ planned |

## Phase B — Guided portfolio project

The separate Data with Baraa portfolio project will be implemented hands-on after the theory block.

Planned sequence:

1. Inspect the messy source model.
2. Understand business entities and table meaning.
3. State the grain before structural changes.
4. Define and build dimensions.
5. Define and build facts.
6. Establish relationships and validate cardinality/filter direction.
7. Add date and role-playing dimensions where required.
8. Define core semantic measures.
9. Implement row-level security.
10. Reconcile baseline metrics and perform final validation.

## Phase C — Independent evidence

After the guided implementation:

- re-audit the model without the tutorial
- justify at least three modeling decisions
- deliberately introduce and diagnose one relationship/modeling failure
- document the root cause, fix and prevention
- reproduce the target model structure from business requirements without step-by-step guidance

## Definition of done

The phase is complete only when I can, without step-by-step guidance:

- determine table grain
- distinguish facts and dimensions
- design a star/galaxy schema
- choose and justify relationship cardinality
- explain filter propagation
- identify risky many-to-many relationships
- detect ambiguous filter paths
- validate that model changes did not break key business metrics
- explain relevant trade-offs and limitations

## Portfolio evidence required

- before/after model diagrams
- source model assessment
- grain matrix
- fact/dimension documentation
- relationship matrix
- baseline and reconciliation tests
- RLS evidence
- architecture decisions
- final validation checklist
- lessons learned and independent audit
