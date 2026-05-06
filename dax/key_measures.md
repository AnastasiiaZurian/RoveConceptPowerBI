## DAX Code - Key Measures

All measures are organized by theme. Each measure includes explanation 
of what it calculates and why it matters for this dashboard.

### Revenue

**Total Revenue**
Core measure — gross revenue across all channels and geographies for the selected period.
```dax
Total Revenue = 
    SUM(FACT_ORDERS[revenue_net])
```

**LY Revenue (Last Year)**
Enables year-over-year comparison by pulling the same period from the prior year.
```dax
LY Revenue = 
    CALCULATE(
        SUM(FACT_ORDERS[REVENUE_NET])
        SAMEPERIODLASTYEAR(DIM_DATE[DATE])
    )
```

**YoY Growth %**
Shows whether each channel or geography is growing or declining compared to last year.
```dax
YoY Growth % = 
    DIVIDE(
       SUM(FACT_ORDERS[REVENUE_NET]) - [LY Revenue], 
        [LY Revenue]
    )
```
### Spend & Efficiency

**Total Spend**
Total marketing investment across all paid channels for the selected period.
```dax
Total Spend = 
    SUM(FACT_MARKETING_SPEND[SPEND_AMOUNT])
```
**MER (Marketing Efficiency Ratio)**
Measures how much revenue the business generates for every $1 of total marketing spend.
```dax
MER = 
    DIVIDE(
       SUM(FACT_ORDERS[REVENUE_NET]),
        SUM(FACT_MARKETING_SPEND[SPEND_AMOUNT])
    )
```

**ROAS (Return on Ad Spend)**
Same logic as MER but scoped to paid channels only — filters out organic and showroom.
```dax
ROAS = 
    DIVIDE(
        FACT_ORDERS[Revenue Net Paid Channels],
       SUM(FACT_MARKETING_SPEND[SPEND_AMOUNT])
    )
```
### Customer Acquisition

**New Customers**
Counts distinct customers whose first-ever order date matches an order placed within 
the selected period. Uses `FILTER` with `RELATED` to cross-reference `fact_orders` 
against each customer's `first_order_date` in `dim_customers` — ensuring only 
genuinely new customers are counted, not repeat buyers.
```dax
New Customers = 
CALCULATE(
    DISTINCTCOUNT(DIM_CUSTOMERS[CUSTOMER_ID]),
    FILTER(
        fact_orders,
        FACT_ORDERS[ORDER_DATE] = RELATED(DIM_CUSTOMERS[FIRST_ORDER_DATE] 
    )
))
```
**CAC (Customer Acquisition Cost)**
How much the business spends on average to acquire one new customer.
```dax
CAC = 
DIVIDE(
    SUM(FACT_MARKETING_SPEND[SPEND_AMOUNT]),
    [New Customers],
    0
)
```

### Showroom Analysis
**Showroom Label**
Calculated column that classifies each geography as Showroom or Non-Showroom based 
on the `showroom_exists` flag in `dim_geo` — used as the axis in all showroom 
comparison visuals.

```dax
Showroom Label = 
    IF(
        DIM_GEO[SHOWROOM_EXISTS] = TRUE(),
        "Showroom",
        "Non-Showroom"
    )
```

**ARPU (Average Revenue Per User)**
Average revenue per customer — used to compare showroom vs. non-showroom performance.
```dax
ARPU = 
   CALCULATE(
            DIVIDE(SUM(FACT_ORDERS[REVENUE_NET]),
     DISTINCTCOUNT(DIM_CUSTOMERS[CUSTOMER_ID])
))
```
