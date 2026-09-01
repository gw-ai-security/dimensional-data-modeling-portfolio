# Lesson 04 — Dimensions Hidden in Facts, Junk Dimensions and Role-Playing Dimensions

**Transcript range:** 01:07:38–01:34:28  
**Source:** Data with Baraa — Data Modeling in Power BI Full Course transcript

## 1. Dimensions can be hidden inside facts

The course now moves from relationship mechanics to identifying descriptive information that has been left inside a fact table.

A fact table normally contains many:

- IDs,
- dates,
- numeric values.

A large block of descriptive text columns inside a fact is therefore treated as a signal to investigate.

The reasoning is:

```text
Descriptive text
→ likely used to describe / filter / group
→ dimension responsibility
```

The goal is not to remove every text field mechanically. The goal is to identify groups of descriptive attributes that represent meaningful analytical context.

## 2. Group related descriptive attributes into a dimension

The transcript uses payment-related columns as an example:

```text
payment_method
card_network
card_type
```

These attributes describe the same business concept and therefore belong naturally together in a Payment dimension.

Other fields such as promotion code, order channel and order status are not grouped into that Payment dimension because they do not describe the same concept.

The modeling rule is:

> Group attributes because they belong to the same business concept, not merely because they are text columns.

## 3. Building a dimension from a fact

The course demonstrates the following preparation pattern in Power Query:

```text
Start from fact
→ keep only descriptive columns for the new dimension
→ remove duplicate combinations
→ add an ID / index for the dimension
→ merge the dimension key back into the fact
→ remove the original descriptive columns from the fact
```

The result is a cleaner fact containing the dimension key rather than repeated descriptive text.

Conceptually:

```text
Before
fact_sales
├── customer_id
├── product_id
├── payment_method
├── card_network
├── card_type
└── sales_amount

After
fact_sales
├── customer_id
├── product_id
├── payment_id
└── sales_amount

              ↓
       dim_payment
       ├── payment_id
       ├── payment_method
       ├── card_network
       └── card_type
```

The dimension combinations are made unique so that the model can support the expected one-to-many relationship.

## 4. Why the key matters

The course does not keep the relationship based on several descriptive text columns. Instead, the extracted dimension gets an ID and the fact receives that ID.

This produces the familiar pattern:

```text
dim_payment 1 ───── * fact_sales
```

The dimension side is unique. The fact side can repeat the dimension key across many business events.

## 5. Junk Dimension

The next problem occurs when a fact contains several small descriptive/flag fields that do **not** belong together as one natural business entity.

Examples in the course include fields such as:

- promotion code,
- order channel,
- order status.

Creating one tiny dimension for every low-level flag would produce many small dimensions around the fact and make the model unnecessarily busy.

The course introduces the **junk dimension** as the solution.

### Definition in the course framing

A junk dimension bundles multiple unrelated low-level flags / small descriptive attributes into one dimension.

```text
promotion_code
order_channel
order_status
        ↓
    dim_flags
```

The point is not that the information is worthless. "Junk" means that the attributes are small, unrelated pieces of analytical context that do not justify separate dimensions.

## 6. Junk-dimension construction pattern

The transcript demonstrates a pattern similar to the previous extracted dimension:

```text
Duplicate/select relevant columns
→ handle null values consistently
→ remove duplicate combinations
→ add a generated ID
→ merge ID back into fact
→ remove original flag columns from fact
```

Handling nulls is important because the composite matching used to recover the dimension key must behave consistently between fact and dimension.

The result is:

```text
dim_flags 1 ───── * fact_sales
```

while the fact becomes narrower and more structurally consistent.

## 7. Why not create one dimension per flag?

The transcript warns that fact tables can contain many status/flag columns. Building one separate dimension for every one-column attribute can create excessive modeling work and clutter the star schema.

The junk dimension reduces that proliferation by bundling suitable low-cardinality flags into one analytical dimension.

## 8. Role-Playing Dimensions

At approximately 01:20 the course introduces another special dimension type: the **role-playing dimension**.

A role-playing dimension is one dimension that can participate in the same fact through multiple business roles.

The example uses Employees in two roles:

```text
Salesperson
Manager
```

The source initially represents them as separate but structurally similar dimensions. Because they describe the same type of entity with the same schema, the course combines them into a generic Employee dimension and preserves the role information.

## 9. Consolidating structurally identical role tables

The demonstrated preparation includes:

- aligning column names;
- adding a role indicator so information is not lost;
- appending the structurally compatible tables;
- renaming the combined table to a generic dimension such as Employee.

Conceptually:

```text
dim_salesperson
       +
dim_manager
       ↓ append after alignment
 dim_employee
```

The important condition is structural compatibility: the source tables represent the same type of entity and can be meaningfully unified.

## 10. Multiple relationships from one dimension to one fact

After consolidation, the Employee dimension plays multiple roles against the Sales fact.

```text
                    ┌─ salesperson_id
 dim_employee ──────┤
                    └─ manager_id
                         fact_sales
```

This means there are multiple relationships between the same dimension and fact.

The course shows that one can be active while another remains inactive to avoid ambiguity.

```text
Employee ID → Salesperson ID  = active
Employee ID → Manager ID      = inactive
```

The same principle applies to a date dimension that serves multiple roles such as Order Date, Ship Date and Delivery Date. Each relationship can still be `1:*`; multiple roles do **not** imply many-to-many cardinality.

## 11. Why alternative role relationships can be inactive

An inactive relationship is not broken and is not defined by `1:1` cardinality. It is a semantically valid alternative relationship that is not the default filter path.

The reason for keeping alternative role relationships inactive is not that multiple active relationships are universally forbidden. The modeling goal is to preserve one unambiguous standard filter path and avoid competing active paths.

Conceptually:

```text
dim_date[date] 1 ─── * fact_sales[order_date]     ACTIVE

dim_date[date] 1 - - * fact_sales[ship_date]     INACTIVE

dim_date[date] 1 - - * fact_sales[delivery_date] INACTIVE
```

## 12. Using an inactive relationship intentionally

By default, Power BI uses the active relationship when the dimension filters the fact.

To calculate something through an alternative role, the course uses a DAX measure with `USERELATIONSHIP` to activate the inactive relationship for that calculation.

The conceptual logic is:

```text
Normal measure
→ active role

Special measure + USERELATIONSHIP
→ alternative inactive role
```

The important modeling lesson is not the DAX syntax itself. It is that one semantic dimension can legitimately represent multiple roles against the same fact without duplicating the entire dimension.

## 13. Role-playing dimension benefits

The source presents this pattern as useful because it:

- reduces duplicated dimensions;
- keeps one consistent representation of the underlying entity;
- supports multiple analytical roles;
- avoids activating multiple competing relationships simultaneously;
- keeps the model more compact and flexible.

## 14. Decision model

```text
Descriptive attributes inside fact?
        │
        ├─ Attributes describe one coherent business concept
        │      → extract a normal dimension
        │
        └─ Several unrelated low-level flags
               → consider a junk dimension

Multiple structurally identical dimensions representing roles?
        │
        └─ consolidate into one role-playing dimension
             + multiple relationships to the fact
             + inactive alternative relationship(s) where needed
```

## 15. Failure modes from this lesson

- leaving large blocks of repeated descriptive attributes in a fact without review;
- grouping unrelated attributes into a normal business dimension as if they described one entity;
- creating one tiny dimension for every low-cardinality flag;
- failing to make extracted dimension combinations unique;
- using inconsistent null handling when generating/matching dimension keys;
- duplicating structurally identical role dimensions unnecessarily;
- assuming alternative role relationships must be inactive because multiple active relationships are generally illegal;
- confusing multiple role-playing relationships with many-to-many cardinality;
- activating multiple competing role relationships and creating ambiguity.

## Active Recall

1. What is the warning sign when a fact table contains many descriptive text columns?
2. How do you decide which descriptive columns belong together in one normal dimension?
3. What preparation steps are used to extract a dimension from a fact?
4. Why is uniqueness important in the extracted dimension?
5. What problem does a junk dimension solve?
6. Why should unrelated low-level flags not necessarily become separate one-column dimensions?
7. Why does consistent null handling matter when building the junk dimension?
8. What is a role-playing dimension?
9. Why can Salesperson and Manager be consolidated into one Employee dimension in the course example?
10. Why is one relationship active and another inactive?
11. What does `USERELATIONSHIP` conceptually allow a measure to do?
12. Why can Order Date, Ship Date and Delivery Date all remain `1:*` relationships to one Date dimension?

## Active Recall checkpoint — 2026-09-01

**Status: completed after correction.**

Correctly recalled:

- dimensions are descriptive analytical context and should be reviewed when descriptive attributes are embedded in a fact;
- a junk dimension bundles heterogeneous low-level descriptive/flag attributes instead of creating many tiny dimensions;
- a role-playing dimension is one dimension serving multiple business roles against the same fact;
- the same Date dimension can represent Order, Ship and Delivery Date roles;
- `USERELATIONSHIP()` can intentionally use an inactive alternative relationship for a specific calculation.

Correction made during recall:

- Alternative role relationships are not inactive because multiple active relationships are universally forbidden. They are kept inactive where necessary to preserve an unambiguous default filter path and avoid competing active paths.

## Learning status

- Theory documentation: ✅
- Lesson watched/studied: ✅
- Active Recall checkpoint: ✅
- Capstone implementation evidence: ⬜
