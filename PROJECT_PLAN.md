# Project Plan

## Goal

Build defensible evidence of dimensional data modeling competence, progressing from course fundamentals to a validated Power BI capstone and an independent model audit.

## Phase A — Theory foundations ✅ COMPLETE

| Module | Evidence | Status |
|---|---|---|
| Why data modeling | `docs/theory/lesson_01_modeling_foundations.md` | ✅ completed |
| Facts vs dimensions | `docs/theory/lesson_01_modeling_foundations.md` | ✅ completed |
| Star / Snowflake / Galaxy | `docs/theory/lesson_02_schema_patterns.md` | ✅ completed |
| Relationships | `docs/theory/lesson_03_relationships.md` | ✅ completed |
| Special dimensions | `docs/theory/lesson_04_special_dimensions.md` | ✅ completed |
| Grain | `docs/theory/lesson_05_grain.md` | ✅ completed |
| Multiple facts | `docs/theory/lesson_06_multiple_facts.md` | ✅ completed |
| Security / RLS | `docs/theory/lesson_07_security_rls.md` | ✅ completed |

A theory module was marked complete only after the corresponding course segment had been watched and explained from memory through Active Recall.

## Phase B — Guided Nightmare portfolio project ▶ CURRENT

The Data with Baraa Nightmare portfolio project will now be implemented hands-on in Power BI. Theory-only status is complete; this phase creates practical evidence.

### B1 — Inspect before changing

1. Inventory all source tables.
2. Understand business meaning and dependencies.
3. Capture the original model state.
4. Record baseline metrics before remodeling.
5. State a grain hypothesis for every fact-like table.
6. Identify duplicate keys, risky relationships and likely data-quality issues.

### B2 — Build dimensions

7. Identify normal dimensions and descriptive attributes hidden in facts.
8. Build/clean dimensions and validate key uniqueness.
9. Use junk dimensions only where the course pattern is justified.
10. Consolidate role-playing dimensions where the business entity is the same.

### B3 — Build facts and relationships

11. State fact grain before transformations.
12. Preserve measures at their correct semantic grain.
13. Decide Append vs Merge vs separate facts based on event, grain and structure.
14. Connect separate facts through shared dimensions rather than direct fact-to-fact relationships.
15. Validate cardinality, filter direction, active/inactive relationships and ambiguity.

### B4 — Semantic layer and security

16. Add Date / role-playing Date relationships where required.
17. Define core semantic measures.
18. Reconcile measures against baseline results.
19. Understand the security requirement before implementation.
20. Implement RLS through a documented filter path.
21. Test representative roles/users and restricted totals.

### B5 — Final validation

22. Reconcile before/after metrics.
23. Capture the final model diagram and evidence screenshots.
24. Complete relationship, security and final validation checklists.
25. Document architecture decisions and trade-offs.

## Phase C — Independent evidence

After the guided implementation:

- re-audit the model without the tutorial;
- justify at least three modeling decisions;
- deliberately introduce and diagnose one relationship/modeling failure;
- document root cause, fix and prevention;
- reproduce the target model structure from business requirements without step-by-step guidance.

## Definition of done

The Data Modeling phase is complete only when I can, without step-by-step guidance:

- determine table grain and distinguish row grain from measure grain;
- distinguish facts and dimensions;
- design a star/galaxy schema;
- choose and justify relationship cardinality;
- explain filter propagation;
- identify risky many-to-many relationships;
- detect ambiguous filter paths;
- recognize when descriptive attributes should become normal or junk dimensions;
- explain and use role-playing dimension patterns;
- choose Append vs Merge vs separate facts based on event, grain and shape;
- compare facts only at a grain both facts understand;
- explain static vs dynamic RLS and trace the security filter path;
- validate that model changes did not break key business metrics;
- explain relevant trade-offs and limitations.

## Portfolio evidence required

- before/after model diagrams;
- source model assessment;
- grain matrix;
- fact/dimension documentation;
- relationship matrix;
- baseline and reconciliation tests;
- RLS evidence;
- architecture decisions;
- final validation checklist;
- lessons learned and independent audit.

## Current gate

**Theory gate: passed.**  
**Implementation gate: not yet passed.**  
**Next action: Issue #6 — audit the Nightmare source model before changing it.**