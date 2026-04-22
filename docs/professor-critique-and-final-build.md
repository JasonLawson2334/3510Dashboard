# Professor-Style Critique + Final Recommended Dashboard (RoR)

This review critiques the current plan as if grading for:
1) usefulness/relevance to management, 2) accuracy/completeness, 3) visual clarity/professionalism.

---

## 1) Professor Critique of the Current Plan

## A. Usefulness & Relevance to Management

### What is strong
- The core business questions are correctly targeted: store performance, flower performance, margin quality, sourcing cost pressure, and collections health.
- KPI set is decision-oriented (Revenue, Purchase Cost, Gross Profit, Gross Margin, Payroll, Misc).
- Store and product visuals are directly tied to operational decisions (pricing, mix, sourcing, staffing).

### What is weak / at risk
1. **Too many optional visuals** can dilute the management story.
   - A one-page executive dashboard should prioritize signal over completeness.
2. **Collections view can be noisy** if receipts are inconsistently posted or dates differ from sales timing.
   - If not clean, this chart may confuse more than help.
3. **Expense breakdown can be redundant** when KPI cards already show Payroll/Misc unless the chart adds trend or store-level context.

## B. Accuracy & Completeness

### What is strong
- Multi-table model and relationship logic are appropriate for the assignment.
- Measures use line-level logic for revenue and purchase cost (better than header-only totals).
- Margin logic is correctly defined as `Gross Profit / Revenue`.

### What is weak / at risk
1. **Revenue vs Purchase Cost trend** can be misleading if dates come from different transaction clocks and are not aligned carefully.
2. **Purchase Cost as COGS proxy** is acceptable for management context, but must be labeled clearly as a proxy if inventory timing differs.
3. **Field inconsistency risk** (`qty`, `quantity`, `qty_sold`, etc.) requires explicit column mapping before finalizing measures.

## C. Visual Clarity & Professionalism

### What is strong
- Layout idea (KPI row + performance row + trend/product row) is suitable.
- Horizontal bars for store/product comparisons are highly readable.

### What is weak / at risk
1. **Six KPI cards + six or seven charts** is likely too dense for one screen.
2. **Top-10 by Revenue plus Top-10 by Profit** can be redundant unless one is tiny and purposefully different.
3. **Too many slicers** (Store + Flower + Grower + Customer + Date) can create clutter and overwhelm.

---

## 2) Charts to Cut / Merge (Weak or Redundant)

Cut first (or move to backup sheet):
1. **Detailed customer collections chart** (if receipts are not cleanly tied to sales periods).
2. **Second flower chart** (keep either Top 10 by Revenue *or* Top 10 by Profit on the main page, not both).
3. **Expense breakdown chart** if it only repeats KPI cards without adding decomposition by store or trend.

Potential merge:
- Merge **Sales by Store** and **Gross Margin by Store** into one visual (combo or two small aligned bars) only if readability remains high.

---

## 3) Visuals Ranked: Most Valuable -> Least Valuable

1. **KPI Cards (Revenue, Purchase Cost, Gross Profit, Gross Margin %, Payroll, Misc)**
2. **Sales by Store**
3. **Sales vs Purchase Cost Monthly Trend**
4. **Top 10 Flowers by Revenue**
5. **Gross Margin by Store**
6. **Collections vs Sales Trend** (only if data quality is high)
7. **Expense Breakdown** (lowest if it duplicates KPIs)

---

## 4) BEST Final One-Page Version (Professor-Preferred)

Target: 4–6 elements + slicers, no clutter.

## Keep these 6 elements (best full version)
1. KPI row (6 cards)
2. Sales by Store (bar)
3. Gross Margin by Store (bar)
4. Sales vs Purchase Cost Trend (line)
5. Top 10 Flowers by Revenue (bar)
6. Collections vs Sales Trend (line) **only if reliable**

## If you must keep only 4–5 elements
- Always keep:
  1) KPI row
  2) Sales by Store
  3) Sales vs Purchase Cost Trend
  4) Top 10 Flowers by Revenue
- Add as 5th element: Gross Margin by Store
- Add as 6th element: Collections vs Sales (only if clean)

---

## 5) Simplified Final Dashboard (De-cluttered)

- Use **3 slicers max** on the main page:
  - Store (required)
  - Date timeline (required)
  - Flower Category or Flower Name (optional, choose one)
- Move Grower and Customer slicers to a secondary analysis sheet.
- Keep one product chart only (Top 10 by Revenue).
- Keep one trend chart for strategic signal (Sales vs Purchase Cost).

---

## 6) How to Make It Look Polished in Excel

## Design rules
- Use one font family (Calibri or Segoe UI).
- Use consistent title format: sentence case, business meaning.
- Use 2 accent colors + neutral gray only.
- Remove chart borders and heavy gridlines.
- Align all objects to an invisible grid; equal spacing.

## Number formatting
- Currency: `$#,##0` (or `$#,##0,"K"` when large)
- Percent: `0.0%`
- Avoid long decimals in labels/axes.

## Chart clarity rules
- Sort bars descending.
- Keep legends only when truly needed.
- Use direct labels for Top 10 bars if space allows.
- Keep axis titles concise and consistent.

## Professional finish checklist
- All titles answer a question (not generic labels).
- No overlapping objects.
- Slicers aligned and same height.
- Dashboard printable on one page / viewable in one screen.

---

## 7) Clean Final Build Steps (Best-Version Workflow)

1. **Map columns first**
   - Build a quick mapping table from actual column names to logical fields (`store#`, `order#`, `inventory#`, `qty`, `price`, `amount`, dates).

2. **Prepare data tables**
   - Convert each range to Excel Table (`Ctrl+T`).
   - Ensure keys are consistent type and dimensions are unique.

3. **Load to Data Model**
   - Add Store, Orders, Order Items, Inventory, Purchases, Purchase Items, Payroll, Misc, Receipts.

4. **Create relationships**
   - Store -> Orders/Purchases/Employees/Misc
   - Orders -> Order Items
   - Inventory -> Order Items and Purchase Items
   - Purchases -> Purchase Items
   - Customers -> Orders/Receipts
   - Employees -> Payroll

5. **Create measures**
   - Revenue, Purchase Cost, Gross Profit, Gross Margin %, Payroll Expense, Misc Expense, Receipt Amount.

6. **Build pivots for final visuals only**
   - P1 KPI source
   - P2 Sales by Store
   - P3 Gross Margin by Store
   - P4 Sales vs Purchase Cost by Month
   - P5 Top 10 Flowers by Revenue
   - P6 Collections vs Sales by Month (optional)

7. **Insert charts and place on Dashboard sheet**
   - Top row: KPI cards
   - Middle: Sales by Store + Gross Margin by Store
   - Bottom: Trend + Top Flowers (+ optional Collections)

8. **Add slicers/timeline**
   - Required: Store + Date timeline
   - Optional: Flower Category/Name

9. **Connect slicers to all pivots**
   - Use Report Connections and test each control.

10. **Final polish + QA**
   - Apply consistent colors/fonts/number formats.
   - Validate totals against raw data.
   - Remove any visual that does not add decision value.

---

## 8) “Professor Pass” Standard

A likely A-level submission:
- Uses 5–6 high-value visuals only
- Has clean relationships and correct measures
- Tells a clear management story in under one minute
- Avoids technical clutter and redundant charts
