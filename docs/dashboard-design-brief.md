# Dashboard Design Brief — RoR Management

## 1) Objective
Create a one-page management dashboard in Excel that allows RoR leaders to monitor performance, identify problem areas quickly, and make decisions about products, locations, and discounts.

## 2) Audience
Primary audience: RoR operations and management stakeholders.

Decisions this dashboard should support:
- Which categories/products should be promoted, deprioritized, or repriced?
- Which stores/regions are outperforming or underperforming?
- Are discount strategies increasing volume at the expense of margin?
- Is monthly performance improving, flat, or declining?

## 3) Required Data Scope
Use multiple related tables (not a single flat table).

Recommended core tables:
- Orders/Transactions
- Products
- Customers
- Stores/Locations
- Calendar/Date dimension (recommended)

## 4) KPI Card Set (Top Row)
Include 5–6 KPI cards with large values and small labels:

1. Total Sales
2. Total Profit
3. Profit Margin %
4. Total Orders
5. Average Order Value (AOV)
6. Year-over-Year Sales Growth % (if prior-year data exists)

Presentation rules:
- Currency: `$#,##0` (or `$#,##0.0K` when needed)
- Percent: `0.0%`
- Include comparison context where possible (e.g., vs prior year)

## 5) Chart Set (Core Visuals)
Use only visuals that answer a management question.

### A. Monthly Sales + Profit Trend (Combo)
- X-axis: Month
- Series 1: Sales (columns)
- Series 2: Profit (line)
- Purpose: Detect momentum and seasonality.

### B. Sales by Product Category (Bar)
- Category-level comparison (sorted descending)
- Purpose: Show revenue concentration.

### C. Profit by Store/Region (Horizontal Bar)
- Rank stores/regions by profit
- Purpose: Identify weak locations quickly.

### D. Top 10 Products by Profit (Bar)
- Filter to top 10 by profit
- Purpose: Focus on high-value items.

### E. Discount vs Margin (Scatter or Grouped Bars)
- Discount rate on one axis, margin/profit on the other
- Purpose: Assess whether discounting hurts profitability.

## 6) Interactivity
Required slicers (connect to all relevant pivots/charts):
- Year
- Quarter/Month
- Region or Store
- Product Category
- Customer Segment (if available)

Optional timeline slicer:
- Date timeline for fast period filtering

## 7) Layout Blueprint (One-Page Dashboard)
Use a clean 12-column-style visual grid.

- Header band (title, subtitle, refresh date)
- KPI card row (uniform size, equal spacing)
- Two chart rows beneath KPIs
- Filter panel either left side or top-right

Visual standards:
- Limit to 2 accent colors + neutrals
- No 3D charts
- Minimal gridlines
- Consistent font (Calibri/Segoe UI)
- Direct, decision-based chart titles

## 8) What Not to Include
- Decorative charts without business purpose
- Duplicative visuals showing the same story
- Unlabeled units or ambiguous abbreviations
- Excessive color variation that reduces readability

## 9) Platform Note
Excel Data Model relationship workflows are fully available in Windows Excel. If using Mac, complete relationship/model steps on a Windows lab computer to satisfy assignment requirements.

## 10) Deliverable Quality Standard
A strong dashboard should be:
- Accurate (numbers reconcile)
- Relevant (management-focused)
- Complete (multiple tables, linked model, clear filters)
- Professional (clean layout, readable labels, consistent formatting)
