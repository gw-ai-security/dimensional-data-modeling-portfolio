# 09 — Security / Row-Level Security Evidence

> **Status: COMPLETE — Dynamic RLS implemented and runtime-validated in Power BI.**

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

`USERPRINCIPALNAME()` identifies the current user; authorization still depends on the mapping table, role expression and valid model relationships.

## Runtime validation

Representative Power BI Desktop `View As` scenarios were executed after the final model refresh.

| Case | Expected behavior | Final result |
|---|---|---|
| user mapped to one Region | only that Region's customer/sales data visible | pass |
| second user / different Region | different allowed Sales scope | pass |
| unmapped user | no customer rows unless a broader rule exists | pass |
| unrestricted view | full recorded Sales baseline restored | pass |

Restricted behavior was reconciled against the expected Region scopes. Real user identities and confidential mappings are intentionally not committed to the public repository.

## Scope caveat

`fact_sales_targets` is period/global data and is not modeled by Region. Regional Sales restricted by RLS must therefore not be interpreted against the global Target as if the target were region-specific.

## Final evidence

- [x] RLS requirement documented
- [x] Dynamic RLS design justified
- [x] role exists in TMDL
- [x] exact DAX rule documented
- [x] security path documented
- [x] representative `View As` scenarios executed
- [x] restricted behavior reconciled
- [x] target/RLS semantic caveat documented
- [x] public evidence sanitized; no real identities committed

The final security claim is limited to this model and test dataset; it is not presented as production IAM or Power BI Service administration evidence.