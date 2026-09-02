# Final Semantic Model — Validated Capstone

> Repository-native summary of the finalized PBIP/TMDL model. Runtime validation and the independent no-tutorial audit are complete.

```mermaid
flowchart LR
    subgraph DIMS[Dimensions]
        DC[dim_customer]
        DP[dim_product]
        DF[dim_order_flags]
        DG[dim_geo]
        DCA[dim_campaign]
        DD[dim_date]
    end

    subgraph FACTS[Facts]
        FS[fact_sales<br/>Order Line]
        FI[fact_inventory<br/>Product-Month]
        FCS[fact_campaign_spend<br/>Campaign-Date]
        FPC[fact_promotion_coverage<br/>Campaign-Product]
        FOP[fact_order_process<br/>Order]
        FST[fact_sales_targets<br/>Period]
    end

    DC --> FS
    DC --> FOP
    DP --> FS
    DP --> FI
    DP --> FPC
    DF --> FS
    DG -->|Ship-To active| FS
    DG -.->|Bill-To inactive| FS
    DCA --> FCS
    DCA --> FPC
    DD --> FS
    DD --> FI
    DD --> FCS
    DD --> FST
    DD -->|Order Date active| FOP
    DD -.->|Ship / Delivery / Invoice / Pay inactive| FOP
```

The final analytical model contains no direct fact-to-fact relationship. Different business events remain separate facts and are compared through shared dimensions at compatible grains.

## Dynamic RLS path

```mermaid
flowchart LR
    U[Current User] --> UPN[USERPRINCIPALNAME]
    UPN --> S[security<br/>user_email + region]
    S --> R[regional access role]
    R --> DC[dim_customer.region]
    DC --> FS[fact_sales]
    DC --> FOP[fact_order_process]
```

Representative `View As` scenarios validated the expected Region-scoped behavior.

## Grain-safety QA

```mermaid
flowchart LR
    O[orders<br/>80 distinct Orders] --> J[Naive child merges]
    J --> BAD[97 rows<br/>grain violation]
    O --> M[Aggregate milestones first]
    M --> GOOD[fact_order_process<br/>80 rows / 80 Orders]
```

## Report proof

```text
Semantic model
→ _measures
→ Business Overview
   ├── Total Sales
   ├── Total Orders
   ├── Active Customers
   ├── Target Attainment
   ├── Sales Trend
   ├── Sales by Product Category
   ├── Sales by Customer Region
   └── Sales vs Target
```

## Technical note

Power BI Auto Date/Time local tables remain serialized. They are technical artifacts, not the intended business calendar. `dim_date` is the explicit analytical date dimension.