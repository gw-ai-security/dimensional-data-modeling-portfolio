# 09 — Security / Row-Level Security Evidence

> **Status: implementation template.** The Security/RLS theory lesson is complete; actual RLS evidence will be added during the guided Nightmare project.

Theory reference: [`theory/lesson_07_security_rls.md`](theory/lesson_07_security_rls.md)

## Security requirement

Document the requirement before changing the model.

**Question:** Who is allowed to see which data?

Record:

- protected business scope;
- authorization attribute (for example Region);
- source of the requirement;
- facts/dimensions that actually require protection;
- whether Static or Dynamic RLS is appropriate.

## Mapping / access model

| Principal / role | Allowed scope | Mapping key | Expected behavior |
|---|---|---|---|
| | | | |

For a public repository, use synthetic identities only.

## Security filter path

Document the exact propagation route, for example:

```text
Current User
→ USERPRINCIPALNAME()
→ Dynamic Role
→ Security Mapping Table
→ Dimension
→ Fact(s)
```

For each hop verify:

- relationship exists;
- cardinality is intentional;
- filter direction allows the security restriction to travel toward protected data;
- the path reaches every fact that is in scope;
- unrelated facts are not forced into the path without a requirement.

## RLS validation cases

| Test user/role | Expected allowed scope | Expected forbidden scope | Expected KPI/total | Actual | Result |
|---|---|---|---:|---:|---|
| | | | | | |

Minimum tests:

1. user with one allowed value;
2. user with multiple allowed values if the mapping supports this;
3. user with no security-table mapping;
4. broad-access role/user if required;
5. verify restricted totals against known baselines.

## Security vs report filtering

```text
RLS           = access boundary
Report filter = analytical selection inside the access boundary
```

Do not treat a slicer/report filter as security evidence.

## Public-repository rules

- do not commit real personal email addresses;
- do not commit credentials, secrets or tokens;
- do not commit confidential organizational access mappings;
- use synthetic principals and synthetic/test mappings;
- screenshots must not expose sensitive identities or internal data.

## Evidence required before marking complete

- [ ] security requirement documented
- [ ] RLS type justified
- [ ] security mapping documented
- [ ] filter path diagram captured
- [ ] representative users/roles tested
- [ ] restricted totals reconciled
- [ ] no unintended data exposure observed
- [ ] public evidence sanitized