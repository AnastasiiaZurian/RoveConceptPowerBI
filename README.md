# Rove Concepts - Executive Marketing Overview Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=for-the-badge&logo=snowflake&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-512BD4?style=for-the-badge&logo=microsoft&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)

This Power BI dashboard delivers a real-time executive view of Rove Concepts' marketing 
performance - tracking revenue, spend efficiency, customer acquisition costs, and geographic 
growth across all channels. 

The dashboard connects directly to a Snowflake data warehouse via a star schema model and 
covers a rolling 2-year period, enabling both year-over-year trend analysis and current 
performance monitoring.

## Table of Content

- [Problem Statement](#problem-statement)
- [Data Source](#data-source)
- [Tools & Tech Stack](#tools--tech-stack)
- [Dashboard Overview](#dashboard-overview)
- [DAX Code — Key Measures](#dax-code--key-measures)
- [Insights](#insights)
- [Recommendations](#recommendations)
## Problem Statement

Rove Concepts is a premium furniture brand operating across the USA, Canada, and emerging 
international markets, running an annual Membership program alongside a multi-channel 
marketing strategy.

With spend distributed across channels (Paid Social, Search, Email, Organic, Host 
Program, and Showroom) and revenue data sitting in Snowflake, there was no fast way to 
answer the questions that mattered most.

The dashboard designed to give executives and marketing managers immediate answers to:

- Which channels are driving the most revenue — and at what efficiency (MER / ROAS)?
- Where are we growing or shrinking year over year, by geography and channel?
- What does it cost to acquire a new customer, overall and by channel?
- Are showroom cities outperforming non-showroom cities on revenue and customer metrics?

The dashboard enables filter by date range, channel, and geography and have every metric update in real time, 
so budget and investment decisions are always grounded in current data.
## Data Source

### Source System
All data is sourced from a **Snowflake data warehouse** using Import mode in Power BI.
The dataset covers a rolling 2-year period and contains ~66,000+ rows across all tables.

### Tables Used in Dashboard #2

| Table | Type | Key Fields | Role |
|---|---|---|---|
| `fact_orders` | Fact | `customer_id`, `channel_id`, `geo_id`, `order_date`, `revenue_gross`, `revenue_net` | Core revenue source |
| `fact_marketing_spend` | Fact | `channel_id`, `date`, `spend_amount`, `clicks`, `impressions` | Channel spend & efficiency |
| `dim_customers` | Dimension | `customer_id`, `first_order_date`, `is_member` | New customer identification |
| `dim_channel` | Dimension | `channel_id`, `channel_name`, `channel_group` | Channel grouping & labeling |
| `dim_geo` | Dimension | `geo_id`, `country`, `region`, `showroom_city`, `showroom_exists` | Geographic segmentation |
| `dim_date` | Dimension | `date`, `month`, `quarter`, `year` | Time intelligence (inactive join) |

### Star Schema

```mermaid
erDiagram
    fact_orders {
        string channel_id FK
        string customer_id FK
        string geo_id FK
        date order_date
        float revenue_gross
        float revenue_net
    }
    fact_marketing_spend {
        string channel_id FK
        date date
        float spend_amount
        int clicks
        int impressions
    }
    dim_channel {
        string channel_id PK
        string channel_name
        string channel_group
    }
    dim_customers {
        string customer_id PK
        date first_order_date
        bool is_member
    }
    dim_geo {
        string geo_id PK
        string country
        string region
        string showroom_city
        bool showroom_exists
    }
    dim_date {
        date date PK
        int month
        string month_name
        int quarter
        int year
    }

    dim_channel ||--o{ fact_orders : "channel_id"
    dim_channel ||--o{ fact_marketing_spend : "channel_id"
    dim_customers ||--o{ fact_orders : "customer_id"
    dim_geo ||--o{ fact_orders : "geo_id"
    dim_date }o--o{ fact_orders : "inactive join"
```

> **Note:** `dim_date` connects to `fact_orders` via an inactive relationship, 
> activated in DAX using `USERELATIONSHIP()` where needed (e.g., New Customers by first_order_date).

### Key Fields for Dashboard #2 Metrics

| Metric | Source Field | Table |
|---|---|---|
| Total Revenue | `revenue_gross` | `fact_orders` |
| Total Spend | `spend_amount` | `fact_marketing_spend` |
| New Customers | `first_order_date` | `dim_customers` + `dim_date` |
| CAC | `spend_amount` / new customer count | `fact_marketing_spend` + `dim_customers` |
| YoY Growth | `revenue_gross` vs. same period last year | `fact_orders` + `dim_date` |
| Showroom Flag | `showroom_exists` | `dim_geo` |
