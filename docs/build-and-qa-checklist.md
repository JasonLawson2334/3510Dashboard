# Build + QA Checklist (Submission-Ready)

## A) Build Checklist

### Data Preparation
- [ ] Raw tables are converted to Excel Tables (`Ctrl+T`)
- [ ] Column names are clear and unique
- [ ] IDs are standardized (no mixed formats)
- [ ] Date columns are valid date types
- [ ] No merged cells in source tables

### Data Model
- [ ] Added all required tables to Data Model
- [ ] Created relationships across multiple tables
- [ ] Confirmed one-to-many direction is correct
- [ ] Tested that dimension filters affect fact metrics

### Dashboard Construction
- [ ] Built all required pivots/charts
- [ ] Added KPI cards with correct formulas
- [ ] Added slicers and connected report connections
- [ ] Applied consistent theme/colors/fonts
- [ ] Dashboard fits one screen without scrolling

## B) Accuracy QA Checklist

- [ ] `SUM(source Sales)` matches dashboard Total Sales
- [ ] `SUM(source Cost)` reconciles with model Cost
- [ ] Profit = Sales - Cost (validated)
- [ ] Margin = Profit / Sales (validated)
- [ ] Date grouping is chronologically correct
- [ ] Top 10 chart truly limited to 10 items
- [ ] Negative values are displayed clearly
- [ ] No blank or "(blank)" categories displayed

## C) Presentation QA Checklist

- [ ] Title clearly states dashboard scope (RoR)
- [ ] Each chart has a direct business-oriented title
- [ ] Units shown (`$`, `%`, counts)
- [ ] Legends are concise and readable
- [ ] Unnecessary chart junk removed
- [ ] Spacing/alignment is visually balanced

## D) Grading Alignment (Self-Check)

### 1) Useful + Relevant for Management
- [ ] Each visual supports a decision question
- [ ] No irrelevant metrics included

### 2) Accurate + Complete
- [ ] Multi-table model is functional
- [ ] All key metrics reconcile to source data
- [ ] Filters/slicers behave correctly

### 3) Professional Visual Presentation
- [ ] Cohesive design language
- [ ] Easy to read in under one minute
- [ ] Clean one-page executive summary layout

## E) Submission Checklist
- [ ] File named `RoR_Dashboard_<YourName>.xlsx`
- [ ] Final sheet named `Dashboard`
- [ ] Workbook saves without broken links
- [ ] All work reflects your individual analysis/design
