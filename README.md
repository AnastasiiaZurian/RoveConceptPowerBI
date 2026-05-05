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
- [Insights & Recommendations](#insights--recommendations)
  
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

<img src="Star_schema.png" width="600"/>

### Tools

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Dashboard development, data modeling, and interactive visualizations |
| **Snowflake** | Cloud data warehouse — single source of truth for all transactional and spend data |
| **DAX** | Calculated measures and KPIs (MER, ROAS, CAC, YoY Growth, New Customers) |
| **SQL** | Data extraction and transformation from Snowflake prior to Power BI import |
| **Power Query** | Data cleaning and shaping within Power BI |

## Dashboard Overview  
### Dashboard in Action
<img src="dashboard_demo.gif" width="700"/>

*Filters update all visuals in real time — switch between years, channels, 
and geographies to instantly compare performance.*

**2024 — Base Year**  
<img src="dashboard_2024.png" width="700"/>

**2025 - Next Year**  
<img src="dashboard_2025.png" width="700"/>

### Layout & Visuals Explained

The dashboard is structured as a single-page layout with 3 filter slicers at the top 
(Year, Channel, Country) and 8 visual sections below.

| Section | Visual Type | What It Shows |
|---|---|---|
| KPI Cards | Cards | Total Revenue, Total Spend, MER, ROAS, New Customers, CAC |
| Revenue and Spend by Channels | Clustered Column Chart | Revenue vs. Spend side-by-side per channel |
| ROAS by Channel | Stackted Bar Chart | Return on ad spend ranked by channel — Email consistently leads |
| Channel Performance Summary | Table | Revenue, Spend, MER, CAC, CPC, CTR per channel in one view |
| New Customers and CAC by Channel |Clustered Column Chart| Acquisition volume vs. cost — identifies most efficient channels|
| YoY Growth % by Country and Channel |Clustered Column Chart| Which markets and channels are growing or shrinking year over year|
| Revenue by Country and Channel | Clustered Bar Chart | Absolute revenue breakdown: USA vs. CA vs. Other, by channel |
| Showroom vs. Non-Showroom |Matrix | Revenue, Customers, and ARPU split by showroom presence |

## Insights & Recommendations

### Key Insights

The following findings are based on a two-year analysis (2024–2025) of Rove Concepts' 
marketing performance across channels, geographies, and customer segments.

- **Revenue scaled significantly while maintaining efficiency.** Total revenue grew 16.5% 
from $54.42M to $63.43M, while MER remained stable at 17.26 — meaning the business 
generated over $17 in revenue for every $1 spent on marketing.

- **Email is the highest-efficiency channel.** With an MER of 19.64 in 2025, Email 
delivered $9.38M in revenue on just $480K in spend — the strongest return of any paid 
channel, yet it remains underleveraged relative to Paid Social.

- **Paid Social drives the most absolute revenue.** Growing from $16.40M to $19.27M YoY 
with a consistent MER of ~10, Paid Social is the volume leader — CTR also improved 
from 3.73% to 5.01%, signaling stronger creative or audience performance.

- **New customer acquisition collapsed in 2025.** Reported new customers dropped from 
6,977 to just 346 — a 95% decline. As a result, blended CAC exploded from $437 to 
$10.62K. The data suggests 2025 revenue was overwhelmingly driven by repeat purchases 
from the existing customer base, pointing to possible market saturation or a data 
tracking issue requiring immediate audit.

- **Non-Showroom channels significantly outperform Showroom on ARPU.** Non-Showroom 
customers generated $4,601 ARPU in 2025 vs. $3,329 for Showroom — a 38% gap — while 
also accounting for the majority of total revenue ($36.8M vs. $26.6M).

- **Emerging international markets show the highest growth velocity.** The "Other" 
geography category dominated YoY growth % across all channels, driven by a low 2024 
baseline. While the USA remains the primary revenue engine, these markets signal 
strong untapped potential.

---

### Recommendations

| # | Finding | Recommendation | Expected Impact |
|---|---|---|---|
| 1 | Email MER of 19.64 — highest of all channels, yet underleveraged | Aggressively scale email list growth and invest in lifecycle marketing sequences | Higher revenue per marketing dollar; reduced reliance on paid channels |
| 2 | New customer acquisition dropped 95% YoY | Immediately audit acquisition tracking for data integrity; if confirmed, launch dedicated re-acquisition campaigns in new geographies | Restored visibility into true CAC; identification of scalable acquisition levers |
| 3 | USA market showing signs of saturation | Shift USA strategy from acquisition to retention and LTV maximization — loyalty programs, personalized re-engagement | Stabilized revenue from core market; improved member retention |
| 4 | "Other" geographies growing at 40–70% YoY across channels | Allocate a dedicated expansion budget to emerging international markets | Accelerated revenue growth outside saturated primary markets |
| 5 | Showroom ARPU ($3,329) significantly trails Non-Showroom ($4,601) | Reevaluate showroom upselling tactics, in-store experience, and real estate ROI | Improved revenue per showroom customer; better justification of physical footprint costs |

---

### Next Steps

1. **Audit new customer data** — investigate the 95% drop in reported new customers 
before making any acquisition budget decisions based on 2025 CAC figures.
2. **Build an email growth roadmap** — given Email's outstanding MER, develop a 
structured plan for list growth, segmentation, and automated lifecycle campaigns.
3. **Model international expansion scenarios** — use the YoY growth data from "Other" 
markets to build a business case for a dedicated 2026 expansion budget.
