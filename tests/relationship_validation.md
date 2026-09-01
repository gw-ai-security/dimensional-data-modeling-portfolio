# Relationship Validation

> **Status: template.**

## Relationship matrix

| Dimension | Fact | Key | Cardinality | Filter direction | Active | Validation result |
|---|---|---|---|---|---|---|
| | | | | | | |

## Checks

- [ ] dimension key is unique where `1` is expected
- [ ] repeated fact keys are understood
- [ ] orphaned fact keys investigated
- [ ] no accidental many-to-many relationships
- [ ] no direct fact-to-fact relationship without explicit justification
- [ ] filter direction matches the intended semantic model
- [ ] no competing active filter paths
- [ ] inactive relationships have documented purpose
