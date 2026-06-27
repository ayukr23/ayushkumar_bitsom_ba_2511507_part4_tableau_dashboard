
## 1. Business Problem Summary

The retail leadership team needs a unified executive dashboard to monitor sales performance, profitability, customer segments, category/sub-category contribution, shipping performance, discount impact, and return patterns across two fiscal years (2024–2025). The goal is to surface business risks and opportunities so that leadership can make data-driven decisions rather than relying on ad-hoc reports.

**Core business questions the dashboard answers:**
- Are sales and profit growing month over month?
- Which regions, categories, and segments drive the most value?
- How does discounting affect profitability?
- Which shipping mode and delivery pattern leads to returns?
- Where are the high-risk sub-categories or segments?

---

## 2. Dataset Description

**File:** `dashboard_sales_data.xlsx`  
**Sheet:** `dashboard_sales_data`  
**Rows:** 4,200 orders | **Columns:** 20 fields  
**Date Range:** 1 January 2024 – 31 December 2025 (2 full fiscal years)

| Field | Type | Description |
|-------|------|-------------|
| order_id | String | Unique order identifier (format: DB-YYYY-######) |
| order_date | Date | Date order was placed |
| ship_date | Date | Date order was shipped |
| customer_id | String | Unique customer identifier |
| customer_segment | Categorical | Consumer / Corporate / Home Office |
| region | Geographic | North / South / East / West |
| state | Geographic | Indian state name |
| city | Geographic | City of delivery |
| category | Categorical | Furniture / Office Supplies / Technology |
| sub_category | Categorical | 13 sub-categories across the 3 categories |
| product_name | String | Individual product name |
| ship_mode | Categorical | Same Day / First Class / Second Class / Standard Class |
| sales | Numeric (₹) | Gross revenue per order |
| quantity | Integer | Units ordered |
| discount | Numeric (%) | Discount applied (0.0 – 1.0 scale) |
| profit | Numeric (₹) | Net profit per order (can be negative) |
| return_flag | Binary | 1 = returned, 0 = not returned |
| delivery_days | Integer | Days between order_date and ship_date |
| customer_rating | Numeric | Customer satisfaction (2.1 – 5.0) |
| campaign_channel | Categorical | Organic / Social / Referral / Paid / Email / (null) |

**Missing Values:**
- `customer_rating`: 32 nulls (0.76%) — treated as missing, excluded from rating averages
- `campaign_channel`: 24 nulls (0.57%) — labeled "Unknown" in campaign analysis

---

## 3. Tableau Workbook Description

**Workbook:** `Retail_Executive_Dashboard.twbx`

The workbook contains **9 sheets** and **1 executive dashboard**:

| Sheet Name | Purpose |
|-----------|---------|
| Sales Trend | Monthly sales and profit trend (2024–2025) |
| Regional Performance | Sales and profit by region and state |
| Category Profitability | Profit by category and sub-category |
| Customer Segment View | Sales, profit, and return rate by segment |
| Shipping Performance | Delivery days, ship mode, and return correlation |
| Discount vs Profit | Scatter plot of discount rate vs profit |
| Return Analysis | Returns by category, segment, and region |
| KPI Summary | Summary cards for Total Sales, Profit, Margin, Return Rate |
| Campaign Performance | Sales by acquisition channel |

---

## 4. Calculated Fields Created

| Field Name | Formula (Tableau syntax) | Purpose |
|-----------|--------------------------|---------|
| `Profit Margin` | `SUM([Profit]) / SUM([Sales])` | Overall profitability ratio |
| `Cost` | `SUM([Sales]) - SUM([Profit])` | Implied cost of goods sold |
| `Average Order Value` | `SUM([Sales]) / COUNTD([Order ID])` | Revenue per unique order |
| `Return Rate` | `SUM([Return Flag]) / COUNT([Order ID])` | Proportion of orders returned |
| `Shipping Delay Bucket` | `IF [Delivery Days] <= 1 THEN "0–1 Days (Express)" ELSEIF [Delivery Days] <= 3 THEN "2–3 Days (Fast)" ELSEIF [Delivery Days] <= 5 THEN "4–5 Days (Standard)" ELSE "6+ Days (Slow)" END` | Bucketed delivery speed |
| `YoY Sales Growth` | `(SUM([Sales]) - LOOKUP(SUM([Sales]), -12)) / ABS(LOOKUP(SUM([Sales]), -12))` | Year-over-year monthly growth |
| `Profitable Order` | `IF [Profit] > 0 THEN "Profitable" ELSE "Loss-Making" END` | Order-level profit flag |

---

## 5. Dashboard Components

The executive dashboard ("Retail Sales Command Center") includes:

**KPI Cards (Row 1):**
- Total Sales: ₹21.70 Cr
- Total Profit: ₹3.33 Cr
- Overall Profit Margin: 15.3%
- Return Rate: 4.5%
- Average Order Value: ₹51,671

**Charts (Rows 2–4):**
1. Sales & Profit Trend (line chart, monthly)
2. Regional Performance (map + bar chart)
3. Category & Sub-Category Profitability (bar chart — sorted by profit)
4. Customer Segment Comparison (grouped bar)
5. Discount vs Profit (scatter plot)
6. Return Analysis by Category (bar chart)
7. Shipping Performance (bar chart by ship mode)

---

## 6. Filters and Interactions Used

| Filter | Type | Applied To |
|--------|------|-----------|
| Region | Dropdown multi-select | All sheets |
| Category | Dropdown multi-select | All sheets |
| Customer Segment | Radio button | All sheets |
| Order Date | Date range slider | Sales Trend, Regional |
| Ship Mode | Checkbox | Shipping Performance, Return Analysis |
| Campaign Channel | Dropdown | Campaign Performance |

**Action Filters:**
- Clicking a region on the map filters all other charts to that region
- Clicking a category bar filters the sub-category and return charts

---

## 7. Key Business Insights

1. **Technology dominates profit** — Technology accounts for 84% of total profit (₹2.80 Cr) despite being only one of three categories. Furniture and Office Supplies contribute marginally.

2. **South region leads sales** — South generates ₹6.47 Cr in sales vs East's ₹4.89 Cr; however, profit margins are comparable across all regions (~15%).

3. **Discounts above 20% destroy profit** — Orders with discounts >30% average a net loss of ₹1,601 per order. Discounts should be capped at 20%.

4. **Furniture has a 7.7% return rate** — Nearly double the overall rate. Tables and Bookcases are the highest-return sub-categories, indicating possible quality or fit issues.

5. **Home Office segment has the highest AOV** — Home Office customers place fewer but larger orders (highest segment sales at ₹7.45 Cr), making them the most valuable retention target.

6. **Same Day shipping has the highest profit margin** — Despite the cost, Same Day orders yield ₹9,102 average profit vs ₹7,839 for Standard Class, suggesting premium customers self-select.

7. **Q3 2025 shows revenue peak** — July–August 2025 saw the highest monthly sales (₹10.66 Cr and ₹10.86 Cr), possibly driven by seasonal or campaign effects. April remains a low point.

8. **Paid and Email campaigns underperform** — Organic and Referral channels generate the highest revenue per order; Paid channel shows below-average profit margins.

---

## 8. Dashboard Story Summary

The dashboard tells a two-year story of a retail business with strong technology-led growth, a regional bias toward the South, and a clear profitability risk from over-discounting in Furniture. While overall return rates are manageable at 4.5%, Furniture's 7.7% rate is a red flag. The business should double down on Technology (especially Copiers and Accessories), protect high-value Home Office customers, and enforce a discount ceiling to stop margin erosion.

---

## 9. Assumptions and Limitations

- `delivery_days` is calculated as ship_date minus order_date, not actual delivery to customer. True last-mile delivery time is unknown.
- Profit is pre-tax and pre-overhead; it reflects gross margin, not net income.
- `return_flag = 1` indicates a return was initiated but does not distinguish between partial and full returns.
- `customer_rating` nulls (n=32) are excluded from all rating calculations; no imputation applied.
- `campaign_channel` nulls (n=24) are labeled "Unknown" and included separately in campaign analysis.
- The dataset covers a synthetic 2-year window; no external benchmarks were applied.
- State-level geographic mapping is based on Indian states (not US states).

---

## 10. Screenshots Included

| File | Description |
|------|-------------|
| `screenshots/full_dashboard.png` | Complete executive dashboard view |
| `screenshots/sales_trend_view.png` | Monthly sales and profit trend chart |
| `screenshots/regional_performance_view.png` | Regional sales and profit (map + bar) |
| `screenshots/category_profitability_view.png` | Category and sub-category profitability |
| `screenshots/filter_interaction_view.png` | Dashboard with filters applied (South region, Technology) |

---
