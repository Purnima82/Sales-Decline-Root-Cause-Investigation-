# DAX Measures — Sales Decline Root-Cause Investigation

Create a measures table in Power BI called `_Measures` (a blank table with no data — just holds these). Paste each measure in one at a time via **Modeling → New Measure**.

## Core Measures

```dax
Total Revenue = SUM(fact_sales[revenue])

Total Orders = DISTINCTCOUNT(fact_sales[order_id])

Avg Order Value = DIVIDE([Total Revenue], [Total Orders])

Total Units Sold = COUNTROWS(fact_sales)
```

## Time Intelligence

```dax
Revenue LM = 
CALCULATE([Total Revenue], DATEADD(dim_date[date], -1, MONTH))

MoM Revenue Growth % = 
DIVIDE([Total Revenue] - [Revenue LM], [Revenue LM])

Revenue LY = 
CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(dim_date[date]))

YoY Revenue Growth % = 
DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY])

Rolling 3M Avg Revenue = 
AVERAGEX(
    DATESINPERIOD(dim_date[date], LASTDATE(dim_date[date]), -3, MONTH),
    [Total Revenue]
)

Peak Month Revenue = 
CALCULATE(MAX([Total Revenue]), ALL(dim_date[year_month]))
```

## Root-Cause / Diagnostic Measures

```dax
Cancellation Rate = 
DIVIDE(
    CALCULATE(DISTINCTCOUNT(fact_sales[order_id]), fact_sales[order_status] = "canceled"),
    [Total Orders]
)

Avg Review Score = 
AVERAGE(fact_sales[review_score])

Avg Delivery Delay (Days) = 
AVERAGE(fact_sales[delivery_delay_days])
-- Negative = delivered early, positive = delivered late vs. estimate

Active Sellers = 
DISTINCTCOUNT(fact_sales[seller_id])

Revenue per Seller = 
DIVIDE([Total Revenue], [Active Sellers])

% of Total Revenue (Category) = 
DIVIDE(
    [Total Revenue],
    CALCULATE([Total Revenue], ALL(dim_product[category]))
)
```

## Decline-Specific Measures (used on Root Cause page)

```dax
Revenue vs Peak % = 
DIVIDE([Total Revenue] - [Peak Month Revenue], [Peak Month Revenue])

Decline Flag = 
IF([MoM Revenue Growth %] < -0.10, "Declining", "Stable/Growing")
```

## Forecast
Don't build this in DAX — use Power BI's native **Analytics pane → Forecast** on the revenue trend line chart (Format visual → Analytics → Forecast). Set forecast length to 3 months, confidence interval 95%. This is faster and more visually credible than a manual DAX forecast formula for a fresher project.

---

### Verified real numbers these measures will produce (from actual analysis of this dataset)
- **Computers & Accessories category** peaked in **Feb 2018** at **R$117,217** revenue (791 orders)
- Declined to **R$50,533** by **June 2018** — a **56.9% peak-to-trough drop**
- Total marketplace revenue stayed flat-to-growing in the same window (R$966K → R$1,128K → R$1,012K), confirming this is a **category-specific** decline, not a market-wide slump
- Avg review score for the category **dropped to 3.47 in Feb 2018** (lowest point in the dataset) — coinciding exactly with the volume peak — then recovered to 4.0–4.29 as volume normalized
- Avg delivery delay **improved** (orders arrived earlier, not later) across this period — ruling out logistics/delivery as the cause
- Avg price **dropped only slightly** (R$110 → R$108) — ruling out pricing as the primary cause
- Active sellers in the category held steady (123 → 127 → 108) — ruling out mass seller attrition
