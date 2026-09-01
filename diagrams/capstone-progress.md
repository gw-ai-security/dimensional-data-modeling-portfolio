# Capstone Progress — Guided Model Build

> Evidence-backed project diagram synchronized with the committed PBIP/TMDL state at video checkpoint **03:45:58**.

```mermaid
flowchart LR
    subgraph STAGE[01_Stage]
        CM[CUST_MASTER]
        CC[customer_contacts]
        UD[user_details]
        AD[Address]
        CI[cities]
        PR[products]
        SC[subcategories]
        O25[ORDERS_2025]
        O26[ORDERS_2026]
        OR[orders]
        CH[channels]
        OLI[order_line_items]
    end

    subgraph DIMS[02_Dimensions]
        DC[dim_customer]
        DP[dim_product]
        DOF[dim_order_flags]
    end

    subgraph FACTS[03_Facts]
        FS[fact_sales - NEXT]
    end

    CM --> DC
    CC --> DC
    UD --> DC
    AD --> DC
    CI --> DC

    PR --> DP
    SC --> DP

    O25 -->|Append| OR
    O26 -->|Append| OR
    OR --> DOF
    CH -->|channel lookup| DOF

    OLI -.->|next guided step| FS
```

## Current interpretation

- `dim_customer` consolidates fragmented customer/context sources into one analytical dimension.
- `dim_product` consolidates product and category/subcategory context and introduces `product_key`.
- `orders` removes the artificial yearly source split while preserving Order grain.
- `dim_order_flags` is a junk dimension at the grain of one unique channel/status/priority combination.
- `channels` is manually entered support data and therefore carries a maintenance/ownership caveat.
- `fact_sales` is shown with a dashed edge because it is the next guided step and **does not yet exist in the committed model at this checkpoint**.

This is a progress diagram, not the final target star/galaxy schema.