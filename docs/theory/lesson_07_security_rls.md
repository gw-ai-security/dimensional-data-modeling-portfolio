# Lesson 07 — Security and Row-Level Security (RLS)

**Transcript range:** 02:08:58–02:40:07  
**Source:** Data with Baraa — Data Modeling in Power BI Full Course transcript

## 1. Security is part of the data model

The course closes the theory block by connecting model quality with access control.

A correct analytical model is not sufficient if users can see data they are not allowed to access. The transcript therefore treats security as deeply connected to data modeling:

```text
Clean model
→ clear filter paths
→ security rules are easier to implement and reason about

Messy model
→ unclear filter paths
→ security is harder to implement safely
→ risk of exposing data to the wrong users
```

The central question becomes:

> **Who is allowed to see which data?**

## 2. Three levels of security in the course

The transcript distinguishes three scopes for hiding data.

### 2.1 Table-level security

Use this concept when an entire table contains information that some users must not access at all.

Examples mentioned in the course include sensitive salary, contact or financial data.

```text
User is not authorized
→ entire table hidden
→ rows and columns unavailable
```

### 2.2 Column-level security

Use this concept when most of a table can be visible but specific columns are sensitive.

Example:

```text
Employee table
├── name          → visible
├── department    → visible
└── salary        → sensitive / hidden
```

The table remains available, but selected sensitive attributes are protected.

### 2.3 Row-level security (RLS)

RLS is used when users may access the table and its columns but only a subset of rows.

Examples from the transcript:

```text
Salesperson → only own region
Manager     → only own department
Director    → all allowed data
```

The course identifies RLS as the most common of the three scenarios and then focuses on it in detail.

## 3. Why separate reports per user group are a weak design

A tempting solution is to build a separate report or model for each region/role.

That repeats the same problem introduced in Lesson 1:

```text
EU report
US report
another regional report
...
```

This increases duplicated maintenance and makes access changes harder to manage.

The RLS objective is instead:

```text
One model / report
+ security rules
→ different users see only the rows they are authorized to see
```

## 4. Static Row-Level Security

The first RLS pattern demonstrated in the course is **static RLS**.

The model contains roles tied to fixed data values.

Example:

```text
Role EU → Region = EU
Role US → Region = US
```

After publishing to Power BI Service, users are manually assigned to the appropriate role.

Conceptually:

```text
User
→ assigned to static role in Power BI Service
→ role contains fixed filter value
→ filter reaches data model
→ user sees permitted rows
```

## 5. Static RLS limitations

The transcript highlights two scaling problems.

### 5.1 Role proliferation

If a security column has many values, a static design can require many roles.

```text
2 regions  → 2 roles
30 values  → potentially 30 roles
```

New values can require additional role maintenance.

### 5.2 Manual user assignment

New users must be manually mapped to roles in Power BI Service.

This becomes impractical as the user population grows.

The course therefore recommends static RLS mainly for smaller, simpler scenarios with relatively few users and values.

## 6. Dynamic Row-Level Security

Dynamic RLS moves user-to-access mapping into data inside the model.

A security table contains information such as:

```text
user_email              region
----------------------  -----------
maria@example.com       Europe
nora@example.com        North America
```

The core architecture becomes:

```text
Current user
    ↓
USERPRINCIPALNAME()
    ↓
one dynamic role / DAX filter
    ↓
security table
    ↓ relationship/filter propagation
dimension
    ↓
fact table(s)
```

## 7. Identifying the current user

The course introduces `USERPRINCIPALNAME()` to identify the user currently opening the report, usually via an email-like identity.

Conceptually:

```text
USERPRINCIPALNAME()
→ current report user
```

That value is then used by the RLS role to filter the security table.

The important point is that the function does not directly filter every fact table. It identifies the user so that the security mapping can be applied.

## 8. The security table is the mapping layer

The dynamic design stores access rules as data rather than as many manually maintained static roles.

```text
User A → Region X
User B → Region Y
User C → Region Z
```

The role filters the security table to the current user. From there, the filter must travel through valid relationships into the rest of the model.

The transcript explicitly emphasizes this sequence:

```text
Current user
→ filter security table
→ relationship
→ analytical dimension(s)
→ fact table(s)
```

Therefore, a security table by itself does not secure the model. The relationship/filter path from that table into the analytical model is essential.

## 9. Static vs Dynamic RLS

| Concern | Static RLS | Dynamic RLS |
|---|---|---|
| Roles | Multiple roles based on fixed values | Usually one dynamic role |
| User mapping | Manually assigned in Power BI Service | Stored in a security table |
| Access change | Change user-role assignment in Service | Change mapping data in security table |
| Simplicity | Easier for small scenarios | More setup initially |
| Scaling | Poorer with many users/values | Better suited to larger/dynamic scenarios |
| Course recommendation | Small projects/few users | Larger projects/many users |

The course focuses more strongly on dynamic RLS because it scales better for the larger project scenarios being taught.

## 10. Security depends on relationship direction

The same relationship principles learned earlier now become security-critical.

If the security table is filtered correctly but the filter cannot propagate into the dimension/fact tables, users may still see data that should have been restricted.

The model must therefore support an intentional security path such as:

```text
security
   ↓
dim_customer
   ↓
fact_sales
```

The dimension is a natural place to apply the security path because dimensions already filter facts in the star-schema design.

## 11. Choose the security attachment point from requirements

The course does not recommend connecting security everywhere by default.

Instead:

1. understand the actual security requirement;
2. locate the business attribute used for authorization, such as Region;
3. identify which dimension(s) contain that attribute;
4. determine which facts must be protected;
5. choose the relationship path that reaches the required scope.

For example, if a Customer dimension filters two relevant fact tables while a Geographic dimension filters only one, Customer may be the better place to attach the security mapping — **if that matches the security requirement**.

The key principle is requirements first, topology second.

## 12. Do not destroy the model to force security everywhere

The transcript explicitly warns against redesigning facts, merging unrelated data, or changing grain simply to push the security attribute into every table.

```text
"Secure everything" without a requirement
→ unnecessary model changes
→ possible grain damage
→ more complexity
```

Security should cover the data that actually requires protection. Tables/dimensions that are harmless to expose do not need to be forced into the same security path.

## 13. Multiple permitted values

The course also considers users who may be authorized for more than one region/value.

The mapping table can contain multiple allowed rows for the same user. The security expression then needs to support membership in the allowed set rather than assuming exactly one value.

Conceptually:

```text
User A → Europe
User A → North America
```

The important modeling idea is that one user can map to multiple permitted business values while the security filter still propagates into the model.

## 14. Testing RLS

Security is not complete until it is tested.

The course uses Power BI's **View as** functionality to test role behavior before handing the model over.

For static RLS, test the configured roles and verify that each sees the expected subset.

For dynamic RLS, test with representative user identities and verify that the security-table mapping produces the intended data visibility.

The validation principle is:

```text
Known unrestricted baseline
→ apply RLS identity / role
→ verify restricted rows and totals
→ test multiple users / roles
```

The transcript repeatedly stresses knowing the important baseline numbers so a security or relationship change cannot silently distort results.

## 15. Security design workflow

```text
1. Understand security requirement
        ↓
2. Identify protected business scope
        ↓
3. Locate authorization attribute in the model
        ↓
4. Choose static or dynamic RLS
        ↓
5. Define security mapping / role
        ↓
6. Verify relationship and filter path
        ↓
7. Test representative users / roles
        ↓
8. Reconcile results against known totals
```

## 16. Failure modes from this lesson

- assuming a correct report is secure by default;
- duplicating reports for every role/region instead of using model security;
- creating a large number of static roles for a highly dynamic access model;
- manually maintaining thousands of user-role assignments when a mapping table is more suitable;
- adding a security table without ensuring its filter reaches the required facts;
- choosing filter direction without considering security propagation;
- forcing security into every fact/dimension without a business requirement;
- changing fact grain merely to add a security attribute;
- failing to test RLS with representative identities;
- validating only that the report opens instead of verifying visible rows and totals.

## Active Recall

1. Why is security treated as part of data modeling in the course?
2. What is the difference between table-, column- and row-level security?
3. Why is creating one report per region/role a weak solution?
4. How does static RLS map users to data?
5. What are the two main scaling problems of static RLS?
6. What does a dynamic security table contain?
7. What role does `USERPRINCIPALNAME()` play?
8. Why does dynamic RLS normally need a valid relationship/filter path from the security table into the model?
9. What is the main administrative difference between static and dynamic RLS when access changes?
10. Why should security requirements be understood before selecting the dimension to secure?
11. Why is it dangerous to modify fact grain simply to force security into every table?
12. How should RLS be tested before the model is considered ready?

## End of theory block

This lesson completes the theory section of the full-course transcript. The transcript then transitions into the guided **Nightmare Data Model** project, where the concepts are applied end to end.

## Learning status

- Theory documentation: ✅
- Lesson watched/studied: ⬜
- Active Recall checkpoint: ⬜
- Capstone implementation evidence: ⬜
