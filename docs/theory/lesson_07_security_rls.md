# Lesson 07 — Security and Row-Level Security (RLS)

**Transcript range:** 02:08:58–02:40:07  
**Source:** Data with Baraa — Data Modeling in Power BI Full Course transcript

## 1. Security is part of data modeling

The course closes the theory block by connecting model design with access control. Security becomes easier to reason about when the model has clear dimensions, facts and filter paths.

Core question:

> **Who is allowed to see which data?**

The transcript explicitly warns that filter direction is security-critical: if the security filter cannot travel from the security mapping into the analytical model, the intended restriction does not happen and data can be exposed incorrectly.

## 2. Three security levels

The transcript distinguishes three levels:

### Table-Level Security

Protect the whole table from unauthorized users.

### Column-Level Security

Keep the table available while protecting specific sensitive columns.

### Row-Level Security (RLS)

Keep the table and columns available but restrict which rows a user may see.

Examples in the course include users limited to their region or department.

```text
Table security  → hide whole table
Column security → hide selected columns
Row security    → hide selected rows
```

RLS is the security type explored in detail in the remainder of the lesson.

## 3. One model with security instead of duplicated reports

Rather than maintaining separate regional/user-specific reports, RLS allows one model/report to expose different authorized row subsets to different users.

```text
One model/report
+ access rules
→ user sees only permitted rows
```

## 4. Static RLS

Static RLS uses fixed rules/values in roles.

Example:

```text
Role Europe → Region = Europe
Role US     → Region = US
```

The course positions this as workable for smaller scenarios with relatively few users/values.

Limitations include:

- multiple roles as access values grow;
- manual role/user administration;
- more maintenance when users or access values change.

## 5. Dynamic RLS

Dynamic RLS moves the user-to-access mapping into data, typically a security table.

Example mapping:

```text
user_email              region
----------------------  -------------
nora@example.com        North America
hans@example.com        Europe
```

Conceptual flow:

```text
Current report user
      ↓
USERPRINCIPALNAME()
      ↓
dynamic role expression
      ↓
security table
      ↓
relationship / filter propagation
      ↓
analytical dimension(s)
      ↓
fact table(s)
```

The transcript emphasizes that the security table is filtered first; Power BI does not simply jump directly from the current user identity to every fact table.

## 6. `USERPRINCIPALNAME()`

`USERPRINCIPALNAME()` identifies the current user opening the report, usually returning an email-like identity.

Its conceptual purpose is:

```text
Who is looking at the report?
→ USERPRINCIPALNAME()
→ match identity to security mapping
```

It is not by itself the complete security mechanism. The identity must be mapped through the role/security table and then propagated through the model.

## 7. Security table as mapping layer

The security table maps users to allowed business values:

```text
User A → Europe
User B → North America
User C → Europe + Middle East
```

A user may have more than one permitted value. The mapping/filter logic must support the allowed set.

If a user has no matching row in the security table, the course demonstrates a deny-by-no-match outcome: the user sees no permitted business rows.

## 8. Relationship and filter direction are security-critical

Dynamic RLS depends on model topology.

Desired pattern:

```text
security table
      ↓
dimension containing authorization attribute
      ↓
fact table(s)
```

If the relationship/filter direction is reversed so that the filter does not travel from security into the model, the protected fact can remain effectively unfiltered. The transcript explicitly frames this as a potential security leak.

Therefore RLS validation must include:

- relationship existence;
- correct cardinality;
- correct filter direction;
- confirmation that the filter reaches every fact that actually requires protection.

## 9. Requirements first

The course advises not to invent security rules based on intuition. The sequence is:

```text
Understand security requirement
→ identify authorization attribute
→ locate it in the model
→ choose where security should attach
→ implement only the required scope
```

The transcript recommends obtaining the actual requirement from the relevant security/business owner rather than randomly applying security across the model.

## 10. Do not damage grain to force security everywhere

The course warns against distorting the analytical model merely to propagate a security attribute into every table.

Security should be applied according to the business requirement and existing semantic model. Facts should not be merged or grain changed simply to make a broad security rule convenient.

## 11. Static vs dynamic RLS

| Concern | Static RLS | Dynamic RLS |
|---|---|---|
| Rule basis | Fixed filter values | Current user + security mapping |
| Roles | Multiple roles may be required | Typically one dynamic role pattern |
| User access mapping | Manual role/user administration | Stored in security table data |
| Change handling | Update role assignment/rules | Update mapping data |
| Course positioning | Small/few-user scenarios | More flexible as project/user count grows |

## 12. Testing RLS

Security is not complete until tested.

The course demonstrates testing the model as different users/roles and verifying the visible regions, rows and totals.

Validation pattern:

```text
Known unrestricted baseline
→ View/Test as role or representative user
→ verify allowed scope
→ verify forbidden scope is absent
→ verify restricted totals
→ test several user mappings
→ test a user with no mapping
```

A report merely opening successfully does not prove that security is correct.

## 13. Security vs report filtering

These concepts have different purposes:

```text
Report filter / slicer
= What does the user want to analyze within allowed data?

RLS
= What data is the user allowed to access at all?
```

RLS defines the permitted row universe; report filtering operates inside that permitted universe.

## 14. Security design workflow

```text
1. Understand security requirement
        ↓
2. Identify protected business scope
        ↓
3. Locate authorization attribute
        ↓
4. Choose static or dynamic RLS
        ↓
5. Define role / security mapping
        ↓
6. Verify relationship + filter path
        ↓
7. Test representative users / roles
        ↓
8. Reconcile restricted results against expected values
```

## 15. Failure modes

- confusing Column-Level Security with Row-Level Security;
- creating many duplicated reports instead of a security model;
- using static role proliferation for a highly dynamic user population;
- adding a security table but failing to connect its filter path to the required facts;
- reversing filter direction and unintentionally exposing unrestricted results;
- assuming `USERPRINCIPALNAME()` alone performs the entire RLS filtering;
- applying security without first understanding the requirement;
- damaging fact grain just to spread an authorization attribute everywhere;
- testing only that the report renders rather than checking allowed/forbidden rows and totals.

## Active Recall checkpoint — 2026-09-01

**Status: completed after clarification.**

Correctly recalled:

- Static RLS uses fixed role/rule mappings; Dynamic RLS uses a user value and security mapping.
- A security/user table maps the current user to the allowed analytical scope.
- Relationships and filter direction are essential because the security filter propagates through the model.
- RLS should be validated by viewing/testing as representative roles/users.
- Security filtering is access control; normal report filtering serves analytical/business exploration.

Clarifications recorded:

1. **Security levels:** `Table / Column / Row`, not `Table / Row / Line`.
2. **`USERPRINCIPALNAME()` semantics:** the function identifies the current report user; it does not independently apply security to every table. The role/security mapping and relationship filter path complete the dynamic RLS design.

## End of theory block

This lesson completes the full pre-project theory section. The next phase is the guided **Nightmare Data Model** project, where the concepts are applied end to end.

## Learning status

- Theory documentation: ✅
- Lesson watched/studied: ✅
- Active Recall checkpoint: ✅
- Complete pre-project theory block: ✅
- Capstone implementation evidence: ⬜ not started