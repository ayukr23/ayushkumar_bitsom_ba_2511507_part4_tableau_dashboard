# Business Insights Report
**Dashboard:** Retail Executive Dashboard  
**Period:** FY2024 – FY2025 | **Orders Analyzed:** 4,200  
**Total Sales:** ₹21.70 Cr | **Total Profit:** ₹3.33 Cr | **Profit Margin:** 15.3%

---

## Calculated Fields Reference

Before insights, here is a precise definition of every calculated field used in the dashboard:

| Field | Tableau Formula | Interpretation |
|-------|----------------|----------------|
| **Profit Margin** | `SUM([Profit]) / SUM([Sales])` | What fraction of each rupee of revenue becomes profit. At 15.3% overall, meaning ₹0.15 net for every ₹1 sold. |
| **Cost** | `SUM([Sales]) - SUM([Profit])` | Implied cost of goods and operations. Useful to spot categories where cost structure is high relative to price. |
| **Average Order Value (AOV)** | `SUM([Sales]) / COUNTD([Order ID])` | Revenue per unique order. Higher AOV = fewer orders needed to hit revenue targets. |
| **Return Rate** | `SUM([Return Flag]) / COUNT([Order ID])` | Proportion of orders that were returned. A rising Return Rate erodes both revenue and customer goodwill. |
| **Shipping Delay Bucket** | `IF [Delivery Days] <= 1 THEN "0–1 Days (Express)" ELSEIF [Delivery Days] <= 3 THEN "2–3 Days (Fast)" ELSEIF [Delivery Days] <= 5 THEN "4–5 Days (Standard)" ELSE "6+ Days (Slow)" END` | Groups delivery speed into four meaningful tiers for operational benchmarking. |

---

## Insight 1 — Sales Trend: Consistent Growth with Seasonal Peaks

**Observation:** Monthly sales show an upward trend from 2024 to 2025, with peaks in July–August and a consistent dip in April each year.

**Data Evidence:**
- July 2025: ₹10.66 Cr | August 2025: ₹10.86 Cr (highest two months in the dataset)
- April 2025: ₹7.19 Cr (lowest month of 2025, ~33% below the August peak)
- 2024 average monthly sales: ~₹8.6 Cr; 2025 average: ~₹9.3 Cr (+8% YoY)

**Business Interpretation:** The July–August peak likely coincides with a festive or back-to-school procurement cycle, especially in Technology. The April trough may reflect post-quarter inventory reset or lower corporate spending.

**Recommended Action:** Pre-load inventory and activate campaign spending by June each year to capture the July–August peak. Introduce April promotions (especially for Office Supplies) to soften the seasonal dip.

---

## Insight 2 — Regional Performance: South Leads, East Lags

**Observation:** The South region generates the highest total sales, while East is the lowest-performing region despite operating in a large market.

**Data Evidence:**
- South: ₹6.47 Cr sales, ₹1.00 Cr profit (Margin: 15.4%)
- North: ₹5.46 Cr sales, ₹0.83 Cr profit (Margin: 15.2%)
- West: ₹4.89 Cr sales, ₹0.74 Cr profit (Margin: 15.1%)
- East: ₹4.89 Cr sales, ₹0.76 Cr profit (Margin: 15.6%)

**Business Interpretation:** All four regions maintain nearly identical profit margins (~15%), suggesting pricing and cost structure are consistent nationally. South's volume advantage is likely driven by market penetration rather than pricing premiums.

**Recommended Action:** Investigate what is driving South's volume advantage (larger sales force? better channel mix? higher urban density?) and replicate it in East and West. Margins are healthy everywhere — the lever is volume, not price.

---

## Insight 3 — Category Profitability: Technology Carries the Business

**Observation:** Technology contributes 84% of total profit despite being just one of three categories.

**Data Evidence:**
- Technology: ₹15.39 Cr sales | ₹2.80 Cr profit (Margin: 18.2%)
- Furniture: ₹5.16 Cr sales | ₹0.36 Cr profit (Margin: 6.9%)
- Office Supplies: ₹1.15 Cr sales | ₹0.17 Cr profit (Margin: 14.8%)
- Top sub-categories: Copiers (₹0.73 Cr), Accessories (₹0.72 Cr), Phones (₹0.71 Cr)

**Business Interpretation:** Furniture's 6.9% margin is dangerously low — it is one discount campaign or supply chain shock away from negative margins. Office Supplies is stable but small. Technology is the clear growth engine.

**Recommended Action:** (1) Shift shelf space and marketing budget toward Technology SKUs, particularly Copiers and Accessories. (2) Audit Furniture pricing and discount policies to improve its margin toward at least 12%. (3) Consider whether low-margin Office Supplies SKUs are needed for basket-building or can be rationalized.

---

## Insight 4 — Customer Segment Behaviour: Home Office is the Silent Winner

**Observation:** All three customer segments show similar total sales (~₹7.0–7.5 Cr each), but Home Office customers place larger, less frequent orders.

**Data Evidence:**
- Home Office: ₹7.45 Cr sales | ₹1.16 Cr profit | Margin: 15.5%
- Consumer: ₹7.19 Cr sales | ₹1.10 Cr profit | Margin: 15.3%
- Corporate: ₹7.06 Cr sales | ₹1.07 Cr profit | Margin: 15.2%
- Average Order Value: Home Office > Consumer > Corporate (based on order frequency and sales volume)

**Business Interpretation:** Home Office customers are likely individual professionals or small business owners who buy Technology in bulk. They require less sales effort per rupee earned and have the highest profit margin. Corporate accounts may be over-supported relative to their margin contribution.

**Recommended Action:** Create a loyalty or account management program for Home Office customers to increase retention. Evaluate whether Corporate account servicing costs are justified by margin. Consider a self-service portal for Corporate to reduce cost-to-serve.

---

## Insight 5 — Discount Impact: Discounting Beyond 20% Destroys Profit

**Observation:** Average profit drops steeply as discount rate increases, turning negative for orders with >30% discount.

**Data Evidence:**
- 0% discount: Avg profit ₹13,203/order
- 1–10% discount: Avg profit ₹10,010/order
- 11–20% discount: Avg profit ₹6,336/order
- 21–30% discount: Avg profit ₹3,181/order
- >30% discount: Avg profit **−₹1,601/order** (loss-making)

**Business Interpretation:** Every 10-percentage-point increase in discount cuts average order profit by ~50%. Orders above 30% discount are net destroyers of value — the business is paying customers to buy. This is particularly acute in Furniture, where margins are already thin.

**Recommended Action:** Implement a hard discount ceiling of 20% in the pricing system. Create an approval workflow for any discount between 15–20%. Remove the ability for sales reps to offer >20% discounts without senior sign-off. Model the profit recovery from enforcing this policy.

---

## Insight 6 — Shipping Performance: Speed Correlates with Higher Profit

**Observation:** Same Day shipping generates the highest average profit per order, not the lowest.

**Data Evidence:**
- Same Day: Avg delivery 0.4 days | Avg profit ₹9,102/order
- Second Class: Avg delivery 2.7 days | Avg profit ₹8,261/order
- Standard Class: Avg delivery 4.7 days | Avg profit ₹7,839/order
- First Class: Avg delivery 1.8 days | Avg profit ₹7,364/order

**Business Interpretation:** Same Day customers are buying premium, higher-ticket Technology items and are less price-sensitive. The premium shipping cost is offset by higher-value orders. Standard Class is the volume backbone but lower per-order profitability.

**Recommended Action:** Upsell Same Day shipping to Technology buyers. Investigate why First Class has lower average profit than Second Class — this may reflect a mismatch between customer type and product category for that ship mode.

---

## Insight 7 — Return Patterns: Furniture is a Return Risk

**Observation:** Furniture has a return rate of 7.7%, nearly double the overall average of 4.5%, indicating quality, fit, or expectation issues.

**Data Evidence:**
- Furniture: 88 returns / 1,140 orders = **7.7% return rate**
- Office Supplies: 61 returns / 1,660 orders = 3.7% return rate
- Technology: 42 returns / 1,400 orders = **3.0% return rate**
- Overall: 191 returns / 4,200 orders = 4.5%

**Business Interpretation:** Furniture returns are costly — reverse logistics, restocking, and customer service cost money the category can't absorb at a 6.9% margin. The combination of low margin AND high return rate makes Furniture the highest-risk category in the portfolio.

**Recommended Action:** (1) Add detailed product descriptions, dimensions, and 360° images to reduce Furniture mis-purchases online. (2) Introduce a "fit guarantee" chat assistant before checkout for Furniture. (3) Audit Tables and Bookcases specifically — the highest-return sub-categories — for quality consistency.

---

## Insight 8 — Business Risk: Furniture Is a Systemic Risk to Profitability

**Observation:** Furniture combines three negative signals: lowest margin (6.9%), highest return rate (7.7%), and heaviest discount dependency. It is the most structurally risky category.

**Data Evidence:**
- Furniture margin: 6.9% vs overall 15.3%
- Furniture return rate: 7.7% vs overall 4.5%
- Furniture discount above 20%: disproportionately high among loss-making orders in scatter analysis
- Tables sub-category: ₹731K profit — healthy on paper, but likely masking high return-adjusted losses

**Business Interpretation:** If shipping and return costs are allocated to Furniture, it is plausible that the category is already margin-negative on a fully-loaded basis. The dashboard's "gross profit" view understates the true risk.

**Recommended Action:** Commission a full landed-cost analysis of Furniture including reverse logistics, warehousing, and customer service. If fully-loaded Furniture is loss-making, leadership should consider (a) dramatic price increases, (b) product line rationalization to premium Furniture only, or (c) exiting the category. This is a board-level risk conversation, not a marketing fix.

---
