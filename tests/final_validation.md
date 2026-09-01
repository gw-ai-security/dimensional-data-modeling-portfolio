# Final Validation Checklist

> **Status: template — complete only after the capstone and independent audit.**

## Model semantics

- [ ] every fact has a precise grain statement
- [ ] every dimension has a precise grain statement
- [ ] fact/dimension classification is justified
- [ ] dimension-side keys expected to be unique are unique
- [ ] relationships match business meaning
- [ ] cardinality is validated
- [ ] filter direction is justified
- [ ] no unexplained many-to-many relationship
- [ ] no accidental direct fact-to-fact relationship
- [ ] no unresolved ambiguous filter path

## Metrics

- [ ] baseline metrics recorded
- [ ] key metrics reconcile after redesign
- [ ] distinct-count requirements match fact grain
- [ ] measure definitions documented

## Security

- [ ] RLS requirement documented
- [ ] synthetic RLS test users/roles validated
- [ ] public repository contains no sensitive identity/credential data

## Evidence

- [ ] before-state model captured
- [ ] final model diagram captured
- [ ] source model assessment complete
- [ ] grain matrix complete
- [ ] fact/dimension documentation complete
- [ ] relationship matrix complete
- [ ] architecture decisions documented
- [ ] debugging/failure evidence documented
- [ ] independent no-tutorial audit complete

## Claim gate

Only after this checklist is complete should the README describe the capstone as a fully implemented and validated modeling project.
