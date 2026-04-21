# Pivot + Data Model Specification (Excel)

Use this as your technical build map while creating the dashboard.

## 1) Data Model Relationships
Create one-to-many relationships from fact table to dimensions:

- `Orders[CustomerID]` -> `Customers[CustomerID]`
- `Orders[ProductID]` -> `Products[ProductID]`
- `Orders[StoreID]` -> `Stores[StoreID]`
- `Orders[OrderDate]` -> `Calendar[Date]` (recommended)

Rules:
- Dimension keys must be unique.
- Fact keys may repeat.
- Date fields must be true date types (not text).

## 2) Calculated Fields / Measures
If using Power Pivot measures, create:

- `Total Sales := SUM(Orders[Sales])`
- `Total Cost := SUM(Orders[Cost])`
- `Total Profit := [Total Sales] - [Total Cost]`
- `Profit Margin % := DIVIDE([Total Profit], [Total Sales])`
- `Order Count := DISTINCTCOUNT(Orders[OrderID])`
- `AOV := DIVIDE([Total Sales], [Order Count])`

Optional YoY measure (requires Calendar table):
- `Sales YoY % := DIVIDE([Total Sales] - CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Calendar[Date])), CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Calendar[Date])))`

If DAX is not available in your class workflow, compute equivalents through PivotTable calculated fields or helper columns.

## 3) PivotTable Specs by Visual

### Pivot 1 — KPI Source
- Values: Total Sales, Total Profit, Profit Margin %, Order Count, AOV, (Sales YoY %)
- Filters: Year, Region, Category, Segment
- Output: linked to KPI card cells

### Pivot 2 — Monthly Trend
- Rows: Calendar[Month] (sorted by month number)
- Values: Total Sales, Total Profit
- Filters: Year, Region, Category
- Chart: Combo (Sales columns + Profit line)

### Pivot 3 — Sales by Category
- Rows: Products[Category]
- Values: Total Sales
- Filters: Year, Region
- Sort: descending by Total Sales
- Chart: Clustered bar

### Pivot 4 — Profit by Region/Store
- Rows: Stores[Region] or Stores[StoreName]
- Values: Total Profit
- Filters: Year, Category
- Sort: descending by Total Profit
- Chart: Horizontal bar

### Pivot 5 — Top 10 Products by Profit
- Rows: Products[ProductName]
- Values: Total Profit
- Value filter: Top 10 by Total Profit
- Filters: Year, Region, Category
- Chart: Bar

### Pivot 6 — Discount vs Margin (if discount exists)
Option A (scatter prep):
- Build summarized table with Product/Category average discount and margin.
Option B (grouped):
- Rows: Discount bands (0–5%, 5–10%, etc.)
- Values: Total Sales, Total Profit or Margin %
- Chart: Clustered columns

## 4) Slicer/Filter Wiring
Connect slicers to all pivots that should synchronize.

Required slicers:
- Year
- Quarter or Month
- Region/Store
- Product Category
- Customer Segment (if available)

Validation:
- Confirm each slicer changes every intended chart.
- Remove stale pivot cache items so old values do not appear.

## 5) Sheet Structure Recommendation
- `Data_*` sheets for raw cleaned tables
- `Pivots` sheet for all pivot tables (can hide)
- `Calc` sheet for helper formulas (optional)
- `Dashboard` sheet for final presentation only

## 6) Number Formatting Standards
- Sales/Cost/Profit: currency with separators
- Percent metrics: 1 decimal place
- Counts: whole numbers with separators
- Axis labels: avoid unnecessary decimals

## 7) Common Accuracy Pitfalls
- Text dates preventing proper month grouping
- Broken relationships from mismatched key types
- Double counting caused by many-to-many errors
- Margin displayed as raw decimal instead of percentage
- Top 10 chart unsorted or not actually filtered
