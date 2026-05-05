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

All data is sourced from a **Snowflake data warehouse** using Import mode in Power BI.
The dataset covers a rolling 2-year period and contains ~66,000+ rows across all tables.

### Tables Used in Dashboard

| Table | Type | Key Fields | Role |
|---|---|---|---|
| `fact_orders` | Fact | `customer_id`, `channel_id`, `geo_id`, `order_date`, `revenue_gross`, `revenue_net` | Core revenue source |
| `fact_marketing_spend` | Fact | `channel_id`, `date`, `spend_amount`, `clicks`, `impressions` | Channel spend & efficiency |
| `dim_customers` | Dimension | `customer_id`, `first_order_date`, `is_member` | New customer identification |
| `dim_channel` | Dimension | `channel_id`, `channel_name`, `channel_group` | Channel grouping & labeling |
| `dim_geo` | Dimension | `geo_id`, `country`, `region`, `showroom_city`, `showroom_exists` | Geographic segmentation |
| `dim_date` | Dimension | `date`, `month`, `quarter`, `year` | Time intelligence (inactive join) |

### Star Schema

![Star Schema](assets/Star_schema.png)
