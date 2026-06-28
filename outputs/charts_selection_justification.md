# Chart Selection Justification
**Dashboard:** Retail Executive Dashboard  

---

## Chart 1 — Sales & Profit Trend (Line Chart)

**Business Question:** Are sales and profit growing over time? Are there seasonal patterns?

**Why This Chart Type:** A line chart is the correct choice for continuous time-series data. The human eye instantly interprets slope (growth/decline) and inflection points (peaks/troughs) on a line. A bar chart would obscure the trend across 24 monthly data points. A scatter plot would lose the sequential time dimension entirely.

**Field Encoding:**
- X-axis: `Order Date` (Month–Year granularity)
- Y-axis: `SUM(Sales)` — primary axis
- Secondary Y-axis: `SUM(Profit)` — dual-axis for direct comparison

**Design Principle Applied:** Dual-axis used carefully — both axes start at zero, ensuring no misleading scale compression. Reference lines added at average monthly sales to show performance context.

**Mistake Avoided:** Did NOT use a stacked area chart — stacking profit on top of sales creates a misleading visual implying they are additive parts of a whole, which they are not (profit is a subset of sales, not an addition to it).

---

## Chart 2 — Regional Performance (Filled Map + Bar Chart)

**Business Question:** Which region generates the most sales and profit? Are there geographic pockets of underperformance?

**Why This Chart Type:** A filled map (choropleth) answers geographic distribution questions immediately — executives can see North/South/East/West performance at a glance without reading labels. A companion horizontal bar chart ranks regions by exact value for precise comparison.

**Field Encoding (Map):**
- Geographic: `State` → mapped to Indian states
- Color intensity: `SUM(Sales)` — darker = higher sales
- Tooltip: Region, Sales, Profit, Profit Margin

**Field Encoding (Bar):**
- Y-axis: `Region` (sorted by Sales descending)
- X-axis: `SUM(Sales)`, with `SUM(Profit)` as a second bar

**Design Principle Applied:** Sorted bar chart (descending by Sales) so the ranking is immediately clear. Map uses a single-color gradient (not diverging) since all values are positive.

**Mistake Avoided:** Did NOT use a pie chart for regional share — pie charts require more than 3–4 slices to be useful, and even then make precise comparison very difficult. The map + bar combination is unambiguous.

---

## Chart 3 — Category & Sub-Category Profitability (Horizontal Bar Chart)

**Business Question:** Which categories and sub-categories drive or destroy profit?

**Why This Chart Type:** A horizontal bar chart sorted by profit value is ideal for comparing many categorical items (13 sub-categories) with long labels. The horizontal orientation prevents label truncation and the sort order makes the ranking immediately obvious.

**Field Encoding:**
- Y-axis: `Sub-Category` (sorted by `SUM(Profit)` descending)
- X-axis: `SUM(Profit)`
- Size: No size encoding — avoids bubble chart complexity for a simple ranking question

**Design Principle Applied:** Color is used to group, not to rank. Diverging color not used because profit values are all positive here. Axis labels include ₹ currency symbol for business readability.

**Mistake Avoided:** Did NOT use a treemap — while treemaps look impressive, they make precise value comparison very hard and can mislead when differences in area are subtle. The ranked bar chart answers "which is better" more directly.

---

## Chart 4 — Customer Segment View (Grouped Bar Chart)

**Business Question:** How do Consumer, Corporate, and Home Office segments compare on sales, profit, and return rate?

**Why This Chart Type:** A grouped bar chart allows simultaneous comparison of multiple measures across a small number of categories (3 segments). It answers both "which segment is largest" and "how do the measures relate within each segment" in one view.

**Field Encoding:**
- X-axis: `Customer Segment` (3 groups)
- Y-axis: `SUM(Sales)` (primary bars)
- Secondary: `SUM(Profit)` as overlapping or adjacent bar
- Color: Measure type (Sales vs Profit)
- Filter: Linked to Region and Category global filters

**Design Principle Applied:** Segments are sorted consistently (Consumer, Corporate, Home Office) across all views in the dashboard for cognitive consistency — the viewer builds a mental model and doesn't need to re-read axis labels each time.

**Mistake Avoided:** Did NOT use a stacked bar chart — stacking would obscure the individual segment values and make the profit component invisible when Sales bars are very tall.

---

## Chart 5 — Discount vs Profit (Scatter Plot)

**Business Question:** What is the relationship between discount rate and profit? Is there a threshold where discounting becomes destructive?

**Why This Chart Type:** A scatter plot is the only chart type that directly shows the relationship (correlation) between two continuous variables — Discount % and Profit ₹. It reveals not just the trend but also the variance: some high-discount orders are still profitable (bulk technology orders), while others are loss-making.

**Field Encoding:**
- X-axis: `Discount` (0 to 1 scale, formatted as %)
- Y-axis: `Profit` (₹)
- Color: `Category` — reveals that Furniture drives most loss-making high-discount orders
- Size: `Sales` — larger bubble = bigger order, making large-loss orders visually prominent
- Reference line: At Discount = 0.20 (the recommended ceiling) and Profit = 0 (break-even line)

**Design Principle Applied:** Two reference lines create four quadrants: the top-left quadrant (low discount, positive profit) is the ideal operating zone, clearly visible without annotation.

**Mistake Avoided:** Did NOT use a line chart for this relationship — a line chart implies sequential ordering, which is inappropriate for bi-variate correlation data. Also avoided a heat map which would lose individual order detail.

---

## Chart 6 — Return Analysis (Bar Chart by Category)

**Business Question:** Which categories, segments, and regions have the highest return rates?

**Why This Chart Type:** A bar chart showing return rate (%) by category is the clearest way to compare a proportion across a small number of groups. Using rate (%) rather than raw count ensures the comparison is fair regardless of category order volume.

**Field Encoding:**
- X-axis: `Category` (or `Sub-Category` in drill-down view)
- Y-axis: `Return Rate` = `SUM([Return Flag]) / COUNT([Order ID])` formatted as %
- Color: Conditional — green (rate < 4%), amber (4–6%), red (>6%) — stops require no legend reading
- Tooltip: Absolute number of returns, total orders, return rate %

**Design Principle Applied:** Color encoding serves a specific purpose here — it alerts leadership to problem categories without requiring them to compare bar heights. This is a traffic-light (RAG) design pattern widely used in executive reporting.

**Mistake Avoided:** Did NOT use a pie chart for return proportion — a pie would imply "returns vs non-returns" are the two halves, which is true but uninformative. The bar chart answers the actionable question: which category needs attention?

---

## Chart 7 — Shipping Performance (Bar Chart by Ship Mode)

**Business Question:** Which shipping mode delivers fastest, and how does delivery speed affect returns?

**Why This Chart Type:** A bar chart comparing four ship modes on average delivery days is simple, unambiguous, and fast to read. An additional bar for return rate per ship mode on a dual axis reveals whether slower shipping drives more returns.

**Field Encoding:**
- X-axis: `Ship Mode` (sorted by avg delivery days)
- Y-axis (primary): `AVG(Delivery Days)`
- Y-axis (secondary): `Return Rate`

**Design Principle Applied:** Sorted by delivery speed (fastest to slowest) so the operational ranking is immediately clear. Ship mode colors are consistent with any other view that uses ship mode to avoid re-learning colors.

**Mistake Avoided:** Did NOT use a timeline or Gantt chart — while theoretically applicable to delivery data, it would require order-level detail that obscures the aggregate pattern leadership needs to see.

---

