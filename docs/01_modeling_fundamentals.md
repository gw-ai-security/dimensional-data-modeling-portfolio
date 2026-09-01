# 01 — Modeling Fundamentals

> Source basis: Data with Baraa Data Modeling full-course transcript. This document paraphrases the concepts covered in the opening section.

## 1. Where data modeling sits

The course places the data model between data preparation and calculations/reporting:

```text
Sources → Power Query / transformation → Data Model → DAX → Reports
```

A visually polished report can still be built on a structurally weak model. The model layer therefore needs deliberate design rather than being treated as a pass-through step.

## 2. Why one large flat table is a weak long-term default

The course starts with the common pattern of collecting everything required for a report into one large table. It may work when the dataset is small, but as the model grows the same descriptive information is repeated across many rows.

The resulting chain is:

```text
repeated information
→ larger model
→ slower refresh/load
→ slower report interaction
```

A weak model can also force calculations to compensate for missing structure, making DAX more complicated than the business calculation itself should be.

## 3. Why one table → one report does not scale

A second problem appears when each report receives its own tailored table/model and duplicated transformation/business logic.

If a business definition changes, the same rule must be updated in several places. A missed update can lead to two reports showing different values for the same KPI.

```text
duplicated logic
→ maintenance effort
→ inconsistent metrics
→ loss of trust in reports
```

The course therefore argues for reusable models around related business areas rather than rebuilding the semantic logic independently for every report.

## 4. What is a data model?

In the course framing, a data model structures business data and the relationships between it. It balances two concerns:

- representing the business meaning of the data
- structuring the data so the analytics tool can use it efficiently and consistently

The model is described through three core components:

1. **Tables** — store/organize the data.
2. **Relationships** — describe how tables connect.
3. **Calculations** — define business metrics and answers.

Useful recall sentence:

> **Tables store. Relationships connect. Calculations answer.**

## 5. Dimensional modeling: facts and dimensions

### Fact table

A fact table records business events, transactions or activities that can be measured.

Typical characteristics shown in the course:

- event/activity rows
- identifiers linking to context
- date/time information
- numeric values used in analysis
- usually many rows

Question to ask:

> **What happened?**

Examples of numeric fact values include quantity, sales, cost, discount or profit depending on the business process.

### Dimension table

A dimension contains descriptive attributes that give a fact context.

Dimensions are used for:

- filtering
- grouping
- labeling/description

Questions to ask:

> **Who? What? When? Where?**

A product dimension might contain product name, category, brand and other descriptive attributes. A customer dimension describes customers rather than recording sales events.

## 6. Measure by context

For a business question such as **Sales by Product Category**:

- `Sales` comes from the fact side / measure logic.
- `Product Category` comes from a dimension attribute and supplies context.

This fact-versus-context distinction is the basis for the schema patterns introduced next.

## Current evidence status

- Concept documented: ✅
- Independent recall performed: ✅
- Capstone implementation: ⬜ not started
- Project-level validation evidence: ⬜ not started
