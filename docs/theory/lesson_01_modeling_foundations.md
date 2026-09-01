# Lesson 01 — Modeling Foundations

**Transcript range:** 00:00:00–00:18:35  
**Source:** Data with Baraa — Data Modeling in Power BI Full Course transcript

## 1. Where data modeling sits in Power BI

The course frames the Power BI workflow as a sequence:

```text
Data sources
→ Power Query / transformations
→ Data model
→ DAX calculations
→ Visuals / reports / dashboards
```

The main warning is that teams often rush through the backend because visuals are more visible and immediately rewarding. Skipping or weakening the modeling step can make the project increasingly heavy, slow, expensive and hard to maintain as data, requirements and users grow.

The core principle of this lesson is therefore:

> A report can look good while the structure underneath it is weak.

## 2. The One Big Flat Table problem

A common first approach is to collect all required columns into one large table and build the report directly on top of it.

This may appear fast when the dataset is small, but the course highlights three scaling problems.

### 2.1 Model size

Descriptive values such as product names and customer names are repeated on every transaction row.

```text
Same descriptive text repeated many times
→ unnecessary storage
→ larger Power BI model
```

The larger the transactional table becomes, the more expensive this repetition becomes.

### 2.2 Performance

The course distinguishes two performance effects:

- **Refresh/load performance:** moving and refreshing increasingly large data takes longer.
- **Report performance:** Power BI has more data to scan when building visuals, applying slicers and reacting to user interactions.

Separating descriptive data into smaller dimensions can reduce repeated data and give Power BI smaller structures to use for filtering.

### 2.3 DAX complexity

A weak or missing model can force DAX to compensate for structural problems.

The course summarizes this relationship as:

```text
Bad / missing model
→ calculations must compensate
→ unnecessarily complicated DAX
```

The point is not that DAX itself is inherently difficult. Some formulas become difficult because the model does not already express the business relationships cleanly.

## 3. The One Table → One Report anti-pattern

The next problem is organizational rather than only technical.

If every new report gets its own tailored flat table, the same transformations and business rules are rebuilt repeatedly:

```text
Report A → custom table A → custom logic A
Report B → custom table B → custom logic B
Report C → custom table C → custom logic C
```

This creates two major risks.

### 3.1 Maintenance duplication

A change to a business rule must be repeated in multiple models/files. Missing one implementation creates divergence.

### 3.2 Inconsistent KPIs

Two reports can begin showing different values for the same business measure because their logic has drifted.

```text
Duplicated logic
→ inconsistent implementation
→ inconsistent numbers
→ users lose trust
```

The course therefore argues for reusable models around related business areas rather than rebuilding the semantic layer independently for every report.

It does **not** argue for one enormous model for the entire company. The target is a sensible shared model around a business/domain context.

## 4. What a data model is

The course defines the model as the structure that describes business data and how the data is related.

It has to satisfy two concerns at the same time:

1. represent the business meaning clearly;
2. be structured efficiently for analytics and Power BI.

Three components are emphasized:

### Tables

Tables organize and store the information used by the model.

### Relationships

Relationships describe how tables are connected and how Power BI can move context/filtering through the model.

### Calculations

Calculations define the business metrics and analytical answers built on top of the structure.

A useful memory model is:

```text
Tables store.
Relationships connect.
Calculations answer.
```

## 5. Dimensional modeling

The course briefly acknowledges that other modeling methods exist, but the Power BI learning path focuses on **dimensional modeling**.

Its two central table roles are:

```text
FACTS + DIMENSIONS
```

## 6. Fact tables

A fact table represents business activity: transactions, events or other things that happened and can be analyzed.

Typical characteristics discussed in the course include:

- many rows,
- identifiers / keys,
- date or time information,
- numeric values,
- business events or transactions.

The primary question is:

> **What happened?**

Examples of fact-like business events include sales or other transactional activities. Numeric columns such as quantity or sales amount are measures recorded at the event's level of detail.

### Important precision

A fact table is not simply "a table with numbers." Its defining idea is the **business event/activity represented by each row**. The numbers are measurements of that event.

## 7. Dimension tables

A dimension describes the context around facts.

Typical dimension attributes are labels and descriptions used to:

- filter,
- group,
- label,
- slice analytical results.

Questions dimensions help answer include:

> **Who? What? Where? When?**

Examples include product name, product category, customer information, store information or descriptive date attributes.

A dimension normally has fewer rows than a large transactional fact and avoids repeating the same descriptive information on every event row.

## 8. Fact versus dimension: Measure by Context

The course uses the analytical pattern **Sales by Category** to make the distinction concrete.

```text
Sales             → fact / numeric measure
Product Category  → dimension / descriptive context
```

This produces the mental model:

```text
MEASURE by CONTEXT
```

Examples:

```text
Sales by Product
Sales by Customer
Sales by Date
Sales by Store
```

The fact provides what is measured. The dimension provides how the measure is grouped or filtered.

## 9. Why separating facts and dimensions helps

The source frames the benefits as a connected chain:

```text
Meaningful tables
+ explicit relationships
→ less repeated descriptive data
→ smaller / cleaner model
→ more efficient filtering
→ simpler calculation logic
→ more reusable semantic structure
```

The purpose is not to split tables arbitrarily. The split must reflect meaningful business roles.

## 10. Lesson decision model

When inspecting a table or column, ask:

```text
Does the row represent a business event/activity?
    └─ Yes → candidate FACT

Does the information describe who/what/where/when?
    └─ Yes → candidate DIMENSION

Is the descriptive information repeated on many event rows?
    └─ Strong signal that it belongs in a dimension
```

## 11. Failure modes from this lesson

- treating one large flat table as a scalable model;
- repeating descriptive text across millions of event rows;
- compensating for poor structure with complicated DAX;
- building one independently tailored data table for every report;
- duplicating business logic until reports disagree;
- confusing numeric columns with the broader concept of a fact table.

## Active Recall

1. Why can a single flat table appear efficient at the beginning but become expensive later?
2. What are the two performance areas affected as a flat model grows?
3. Why can a bad model make DAX unnecessarily complicated?
4. Why does One Table → One Report create a trust problem for the business?
5. What are the three main components of a data model in the course?
6. What question does a fact answer?
7. What questions does a dimension help answer?
8. In `Sales by Product Category`, which element comes from the fact side and which from the dimension side?
9. Why is "facts are numbers" an incomplete definition?

## Learning status

- Theory documentation: ✅
- Lesson watched/studied: ✅
- Active Recall checkpoint: ✅
- Capstone implementation evidence: ⬜
