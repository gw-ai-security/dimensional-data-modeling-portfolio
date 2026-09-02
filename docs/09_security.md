# 09 — Security / Row-Level Security Evidence

> **Status: dynamic RLS implemented in TMDL; representative Power BI runtime tests still pending.**

Theory reference: [`theory/lesson_07_security_rls.md`](theory/lesson_07_security_rls.md)

## Requirement

Restrict customer-related analytical data by the Region(s) assigned to the current user.

## Implementation

The model contains:

- `security[user_email]`
- `security[region]`
- role `regional access`
- `dim_customer[region]` as the controlled analytical attribute

The source-controlled role expression is:

```DAX
dim_customer[region] IN
CALCULATETABLE(
    VALUES(security[region]),
    security[user_email] = USERPRINCIPALNAME()
)
```

## Security path

```text
Current Power BI user
→ USERPRINCIPALNAME()
→ security[user_email]
→ allowed security[region]
→ role filter on dim_customer[region]
→ customer-related facts through dimensional relationships
```

`USERPRINCIPALNAME()` identifies the current user; it is not itself the complete authorization model.

## Scope caveat

Not every fact has Region grain. In particular, `fact_sales_targets` is period-based/global in this dataset. Regional Sales restricted by RLS must not automatically be compared to a global Target as if the target were regional.

## Runtime test plan

Power BI Desktop `View As` must still be executed with representative synthetic/test users.

| Case | Expected result | Status |
|---|---|---|
| user mapped to one Region | only that Region's customer/sales data visible | pending runtime evidence |
| second user / different Region | different allowed Sales scope | pending runtime evidence |
| unmapped user | no customer rows unless an explicit broader rule exists | pending runtime evidence |
| unrestricted view | full recorded Sales baseline restored | pending runtime evidence |

## Evidence boundary

- [x] RLS requirement documented
- [x] dynamic design justified
- [x] role exists in TMDL
- [x] exact DAX rule documented
- [x] security path documented
- [x] target/RLS semantic caveat documented
- [ ] representative users tested with `View As`
- [ ] restricted totals reconciled
- [ ] sanitized RLS screenshot committed

A source-controlled role is implementation evidence, not proof that authorization has been runtime-tested.