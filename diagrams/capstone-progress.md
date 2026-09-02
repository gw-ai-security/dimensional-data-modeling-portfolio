# Capstone Implementation Map — Current Model

> Repository-native summary of the current source-controlled guided implementation. Final runtime validation remains separate.

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

## Security path

```mermaid
flowchart LR
    U[Current User] --> UPN[USERPRINCIPALNAME]
    UPN --> S[security<br/>user_email + region]
    S --> R[regional access role]
    R --> DC[dim_customer.region]
    DC --> FS[fact_sales]
    DC --> FOP[fact_order_process]
```

## Grain-safety QA

```mermaid
flowchart LR
    O[orders<br/>80 distinct Orders] --> J[Naive child merges]
    J --> BAD[97 rows<br/>grain violation]
    O --> M[Aggregate milestones first]
    M --> GOOD[fact_order_process<br/>1 row per Order]
```

## Known technical limitation

Power BI Auto Date/Time local tables are still serialized. `dim_date` is the intended analytical calendar; Auto Date artifacts are not represented as business dimensions in this diagram.