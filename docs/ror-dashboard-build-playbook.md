# RoR Professional Dashboard Build Playbook (Windows Excel + Data Model)

This playbook is written for your **actual RoR assignment context**: multi-store flower distributor, multi-table model, management-focused visuals, and one-page dashboard.

---

## PHASE 1 — Data Model Inspection + Recommended Relationship Map

Because workbook/table names can vary (`Store`, `Stores`, `Str`; `Order Items`, `OrderItems`, etc.), use this matching rule:

- `store#`, `store_id`, `storeid` => same key role
- `customer#`, `cust#`, `customer_id` => same key role
- `inventory#`, `item#`, `flower#`, `product#` => same key role (flower/item dimension)
- `employee#`, `emp#` => same key role
- `purchase#`, `po#` => same key role
- `order#`, `invoice#`, `sales#` => same sales header key role

### Relationship map (recommended)

| Parent Table | Parent Key | Child Table | Child Key | Why this matters |
|---|---|---|---|---|
| Store | store# | Orders | store# | Enables sales by store and store-level profitability. |
| Store | store# | Purchases | store# | Enables purchase cost analysis by store. |
| Store | store# | Employees | store# | Enables labor analysis by store. |
| Store | store# | Misc | store# | Enables admin/misc expense by store. |
| Customers | customer# | Orders | customer# | Enables customer-level sales and collections comparisons. |
| Orders | order# | Order Items | order# | Enables line-level revenue and quantity calculations. |
| Inventory (Flowers/Items) | inventory# | Order Items | inventory# | Enables top flowers and flower profitability. |
| Growers | grower# | Purchases | grower# | Enables source/grower purchase trend analysis. |
| Purchases | purchase# | Purchase Items | purchase# | Enables line-level purchase cost calculations. |
| Inventory (Flowers/Items) | inventory# | Purchase Items | inventory# | Enables purchase cost by flower comparisons. |
| Employees | employee# | Time Cards | employee# or emp# | Enables labor-hour and productivity context (optional). |
| Employees | employee# | Payroll | employee# or emp# | Enables payroll expense by employee/store. |
| Customers | customer# | Receipts | customer# | Enables sales vs collections insight. |
| Purchases | purchase# | Checks (optional) | purchase# | Enables AP disbursement linkage (optional for dashboard). |

### Ambiguous relationships / optional tables

- **Trucks**: usually operational/logistics only; include only if freight cost materially affects profitability.
- **Bank Accounts**: useful for treasury/cash management, usually not needed for this management dashboard.
- **Checks**: use only if you need AP payment timing; otherwise Purchases + Purchase Items already covers sourcing cost trend.
- **Time Cards**: optional unless management wants labor hours KPIs (not required for your core scope).

> If you hit many-to-many warnings, stop and confirm the parent table key is unique (remove duplicates or build a distinct dimension table).

---

## PHASE 2 — Best One-Page Dashboard Content (Management-Focused)

### A) KPI cards (top row)
1. **Total Revenue**
2. **Total Purchase Cost (COGS proxy)**
3. **Gross Profit**
4. **Gross Margin %**
5. **Payroll Expense**
6. **Misc/Admin Expense**

### B) Sales by Store
- Horizontal bar chart (fast comparison, readable store names).
- Value: Revenue (optionally add Gross Profit as second series).

### C) Top Flowers
- Top 10 Flowers by Revenue (primary chart).
- If space permits: Top 10 by Gross Profit (secondary small chart).

### D) Sales vs Purchase Cost Trend
- Monthly line chart with both metrics.
- Management question answered: Are costs rising faster than sales?

### E) Gross Margin by Store
- Bar chart with margin % by store (sorted descending).

### F) Expense Breakdown
- Small horizontal bars: Payroll, Misc/Admin, Purchase Cost.
- Keep simple (avoid cluttered donut unless very few categories).

### G) Collections view (include only if clean)
- Monthly Sales vs Receipts line chart.
- If unreliable joins or missing dates, drop this chart before final submission.

---

## PHASE 3 — Exact Measures / Calculations (Power Pivot)

Create these measures in Data Model (Power Pivot > Manage > Measures).

### Sales + product measures
```DAX
Revenue :=
SUMX(
    'Order Items',
    COALESCE('Order Items'[qty_sold], 'Order Items'[quantity]) *
    COALESCE('Order Items'[sale_price], 'Order Items'[unit_price])
)

Quantity Sold :=
SUMX('Order Items', COALESCE('Order Items'[qty_sold], 'Order Items'[quantity]))
```

### Purchase / cost measures
```DAX
Purchase Cost :=
SUMX(
    'Purchase Items',
    COALESCE('Purchase Items'[qty_purchase], 'Purchase Items'[quantity]) *
    COALESCE('Purchase Items'[purchase_price], 'Purchase Items'[unit_cost])
)

Quantity Purchased :=
SUMX('Purchase Items', COALESCE('Purchase Items'[qty_purchase], 'Purchase Items'[quantity]))
```

### Profitability measures
```DAX
Gross Profit := [Revenue] - [Purchase Cost]

Gross Margin % := DIVIDE([Gross Profit], [Revenue])

Profit per Flower := DIVIDE([Gross Profit], [Quantity Sold])
```

### Expense / cash measures
```DAX
Payroll Expense := SUM('Payroll'[amount])

Misc Expense := SUM('Misc'[amount])

Receipt Amount := SUM('Receipts'[amount])
```

### Diagnostic / supporting measures
```DAX
Sales per Customer := DIVIDE([Revenue], DISTINCTCOUNT(Orders[customer#]))

Sales per Store := DIVIDE([Revenue], DISTINCTCOUNT(Store[store#]))
```

### If your columns are named differently
Swap the field references to your exact column names. Keep measure names unchanged for consistent pivot building.

### Date strategy (very important)
Use separate date dimensions if needed:
- `Calendar_Sales` linked to `Orders[order_date]`
- `Calendar_Purchases` linked to `Purchases[purchase_date]`
- `Calendar_Receipts` linked to `Receipts[receipt_date]`

If you prefer one calendar table, you can keep one **active** relationship and use `USERELATIONSHIP()` measures for other date fields.

---

## PHASE 4 — Exact Excel Build Steps

### 1) Clean / prep
- Standardize key columns as text or whole number consistently.
- Remove blank key rows and duplicate dimension keys.
- Ensure all date columns are true date type.

### 2) Convert ranges to tables
- For each dataset: select range > **Ctrl+T** > check *My table has headers*.
- Name tables clearly: `Store`, `Orders`, `OrderItems`, `Purchases`, `PurchaseItems`, etc.

### 3) Add tables to Data Model
- Power Pivot tab > **Add to Data Model** for each table.

### 4) Create relationships
- Data tab > Relationships (or Power Pivot Diagram View).
- Build all relationships from the map above.
- Ensure one-to-many direction is valid.

### 5) Create measures
- In Power Pivot, add all measures from Phase 3.
- Format: currency for money, percentage for margin.

### 6) Build pivot tables (exact field layout)

#### Pivot P1 — KPI source
- Rows: none
- Columns: none
- Filters: optional Year/Store
- Values: Revenue, Purchase Cost, Gross Profit, Gross Margin %, Payroll Expense, Misc Expense
- Chart: none (linked cells for KPI cards)

#### Pivot P2 — Sales by Store
- Rows: Store name
- Columns: none
- Filters: Year, Flower Category (if available)
- Values: Revenue (optional Gross Profit)
- Sort: descending Revenue
- Chart: horizontal clustered bar

#### Pivot P3 — Top 10 Flowers
- Rows: Flower name (Inventory name)
- Columns: none
- Filters: Year, Store
- Values: Revenue (or Quantity Sold)
- Sort: descending Revenue
- Top N: Top 10 by Revenue
- Chart: bar

#### Pivot P4 — Sales vs Purchase Cost trend
- Rows: Month-Year (from sales calendar)
- Columns: none
- Filters: Store, Grower (optional)
- Values: Revenue, Purchase Cost
- Sort: ascending month
- Chart: line chart (2 series)

#### Pivot P5 — Gross Margin by Store
- Rows: Store name
- Columns: none
- Filters: Year
- Values: Gross Margin %
- Sort: descending Gross Margin %
- Chart: bar

#### Pivot P6 — Expense breakdown
- Rows: Expense Type (or use separate measures)
- Columns: none
- Filters: Year, Store
- Values: Payroll Expense, Misc Expense (optionally Purchase Cost)
- Sort: descending value
- Chart: horizontal bar

#### Pivot P7 — Collections vs Sales (optional, recommended if clean)
- Rows: Month-Year (receipt/sales aligned by month)
- Columns: none
- Filters: Store, Customer segment (if available)
- Values: Revenue, Receipt Amount
- Sort: ascending month
- Chart: line chart

### 7) Insert pivot charts
- Insert chart from each pivot and move to `Dashboard` sheet.

### 8) Add slicers/timeline
- Insert slicers for Store, Flower, Month, Grower.
- Optional: Customer slicer only if not too long.
- Add Timeline for month/date.

### 9) Connect slicers
- Click slicer > Report Connections > check all relevant pivots/charts.
- Test each slicer updates all intended visuals.

### 10) Format + arrange
- Hide field buttons on charts.
- Standardize titles, fonts, colors, and number formats.
- Align all objects to grid and equal spacing.

---

## PHASE 5 — Slicer Recommendations

### Definitely include
- **Store** (core management cut)
- **Month/Date timeline** (trend context)
- **Flower Name or Category** (product mix)

### Optional (include only if useful)
- **Grower** (if management monitors sourcing risk/cost)
- **Customer** (only if filtered list is manageable)

### Usually skip
- Employee slicer on main dashboard (too detailed for exec view)

---

## PHASE 6 — One-Page Visual Layout Plan

### Suggested placement grid
- **Top row (full width):** 6 KPI cards
- **Middle left:** Sales by Store
- **Middle center:** Gross Margin by Store
- **Middle right:** Expense Breakdown
- **Bottom left (wide):** Sales vs Purchase Cost Trend
- **Bottom center/right:** Top 10 Flowers
- **Bottom far right (small):** Collections vs Sales (optional)
- **Right sidebar or top-right:** slicers + timeline

### Formatting standards
- Palette: Navy `#1F3A5F`, Teal `#2C7A7B`, Gray `#6B7280`, Light background `#F8FAFC`.
- Revenue/positive = navy/teal; costs = gray/orange accent.
- Currency: `$#,##0` or `$#,##0,"K"` for thousands.
- Percent: `0.0%`.
- Remove heavy gridlines and unnecessary legends.
- Keep titles action-oriented:
  - “Sales by Store (YTD)”
  - “Are Purchase Costs Outpacing Sales?”
  - “Top 10 Flowers Driving Revenue”

---

## PHASE 7 — Final Grading Checklist

### Must-pass checks
- [ ] Uses multiple tables in Data Model (not single-table pivots)
- [ ] Relationships correctly created and functioning
- [ ] KPI logic ties to transactional data
- [ ] Visuals directly answer management questions
- [ ] Trend chart uses correctly grouped Month-Year dates
- [ ] Dashboard is readable and professional on one page
- [ ] No filler/decorative chart clutter
- [ ] Story is clear: sales, cost, profit, expense, collections

### Common mistakes to avoid
1. Using `Orders total` while ignoring line-item detail in `Order Items`.
2. Comparing sales month to purchase month from mismatched date filters.
3. Building margin from already-aggregated values incorrectly.
4. Leaving unconnected slicers (some charts don’t change).
5. Using pie/donut charts with too many categories.

### If page is crowded, cut in this order
1. Customer-level collections detail chart (keep only monthly collections vs sales).
2. Secondary top-flowers chart (keep only one top-10 view).
3. Grower-sliced view (keep as slicer, not separate chart).

### Highest-value visuals that must stay
1. KPI row (Revenue, Cost, Gross Profit, Margin, Payroll, Misc)
2. Sales by Store
3. Sales vs Purchase Cost monthly trend
4. Top Flowers (Revenue)
5. Gross Margin by Store

These five elements are the strongest grading-safe core.
