## DAX Code — Key Measures

All measures are organized by theme. Each measure includes a plain-English explanation 
of what it calculates and why it matters for this dashboard.

---

### Revenue

**Total Revenue**
Core measure — gross revenue across all channels and geographies for the selected period.
```dax
Total Revenue = 
    SUM(FACT_ORDERS[revenue_gross])
```

**Revenue LY (Last Year)**
Enables year-over-year comparison by pulling the same period from the prior year.
```dax
Revenue LY = 
    CALCULATE(
        [Total Revenue],
        SAMEPERIODLASTYEAR(DIM_DATE[date])
    )
```

**YoY Growth %**
Shows whether each channel or geography is growing or declining compared to last year.
```dax
YoY Growth % = 
    DIVIDE(
        [Total Revenue] - [Revenue LY],
        [Revenue LY]
    )
```

---

### Spend & Efficiency

**Total Spend**
Total marketing investment across all paid channels for the selected period.
```dax
Total Spend = 
    SUM(FACT_MARKETING_SPEND[spend_amount])
```

**MER (Marketing Efficiency Ratio)**
Measures how much revenue the business generates for every $1 of total marketing spend.
```dax
MER = 
    DIVIDE(
        [Total Revenue],
        [Total Spend]
    )
```

**ROAS (Return on Ad Spend)**
Same logic as MER but scoped to paid channels only — filters out organic and showroom.
```dax
ROAS = 
    DIVIDE(
        CALCULATE(
            [Total Revenue],
            DIM_CHANNEL[channel_group] = "Paid"
        ),
        CALCULATE(
            [Total Spend],
            DIM_CHANNEL[channel_group] = "Paid"
        )
    )
```

---

### Customer Acquisition

**New Customers**
Counts distinct customers whose first-ever order falls within the selected date range.
Uses `USERELATIONSHIP` to activate the inactive join between `dim_date` and 
`dim_customers[first_order_date]`.
```dax
New Customers = 
    CALCULATE(
        DISTINCTCOUNT(DIM_CUSTOMERS[customer_id]),
        USERELATIONSHIP(
            DIM_CUSTOMERS[first_order_date],
            DIM_DATE[date]
        )
    )
```

**CAC (Customer Acquisition Cost)**
How much the business spends on average to acquire one new customer.
```dax
CAC = 
    DIVIDE(
        [Total Spend],
        [New Customers]
    )
```

**Total Customers**
Distinct count of all customers who placed at least one order in the selected period.
```dax
Total Customers = 
    DISTINCTCOUNT(DIM_CUSTOMERS[customer_id])
```

---

### Showroom Analysis

**ARPU (Average Revenue Per User)**
Average revenue per customer — used to compare showroom vs. non-showroom performance.
```dax
ARPU = 
    DIVIDE(
        [Total Revenue],
        [Total Customers]
    )
```
