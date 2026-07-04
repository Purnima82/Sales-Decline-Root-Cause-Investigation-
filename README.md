# Sales Decline Root-Cause Investigation | Power BI

## 📌 Business Problem
[Company/Category] experienced a [X]% revenue decline in [Category] within [Region] during [Quarter/Month, Year]. This project investigates the root cause using real e-commerce transaction data and delivers data-backed recommendations to reverse the trend.

## 📊 Dataset
- **Source:** [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Size:** 100,000+ real orders (2016–2018)
- **Tables used:** Orders, Order Items, Products, Customers, Sellers, Payments, Reviews, Category Translation
- **Note:** This is real transactional data from an actual Brazilian marketplace. The decline investigated is a genuine pattern found in the data, analyzed independently (not a live business engagement).

## 🗂️ Data Model
Star schema design:
- **Fact table:** Orders/Sales (order_id, product_id, customer_id, seller_id, date_id, revenue, quantity)
- **Dimension tables:** Date, Product/Category, Customer, Geography, Seller

![Star Schema](screenshots/star_schema.png)

## 🧮 Key DAX Measures
```dax
Total Revenue = SUM(Orders[revenue])

YoY Growth % = 
DIVIDE(
    [Total Revenue] - CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date])),
    CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))
)

MoM Growth % = 
DIVIDE(
    [Total Revenue] - CALCULATE([Total Revenue], DATEADD('Date'[Date], -1, MONTH)),
    CALCULATE([Total Revenue], DATEADD('Date'[Date], -1, MONTH))
)

Rolling 3-Month Avg Revenue = 
AVERAGEX(
    DATESINPERIOD('Date'[Date], LASTDATE('Date'[Date]), -3, MONTH),
    [Total Revenue]
)

Cancellation Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(Orders), Orders[order_status] = "canceled"),
    COUNTROWS(Orders)
)

Avg Delivery Delay (Days) = 
AVERAGEX(Orders, DATEDIFF(Orders[estimated_delivery_date], Orders[actual_delivery_date], DAY))
```
*(Full measure list in the .pbix file / Model view)*

## 📄 Report Pages
1. **Executive Overview** — KPI cards, revenue trend with forecast overlay, YoY/MoM summary
2. **Root Cause – Decomposition Tree** — automated breakdown of decline by category, region, and time period
3. **Drill-Through Detail** — category/region-level drill into product and seller performance
4. **Recommendations** — data-backed actions with quantified revenue impact

## 🔍 Key Findings
- [Finding 1 — e.g., Category X in Region Y declined Z% in Quarter Q]
- [Finding 2 — e.g., root cause identified: delivery delays / pricing shift / competitor category growth]
- [Finding 3 — supporting metric]

## ✅ Recommendations
1. [Recommendation 1 with quantified impact]
2. [Recommendation 2 with quantified impact]
3. [Recommendation 3 with quantified impact]

## 🔗 Live Dashboard
[Power BI Service link]

## 🛠️ Tools Used
Power BI · DAX · Power Query · Data Modeling (Star Schema)

## 📁 Repository Structure
```
├── data/               # Raw and cleaned CSVs (or link to source)
├── sales_decline.pbix  # Power BI file
├── screenshots/         # Dashboard page screenshots + model diagram
├── insight_report.pdf   # 1-page summary report
└── README.md
```
