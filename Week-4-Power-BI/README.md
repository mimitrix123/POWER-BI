# Week 4 – Dashboard Design & Publishing

## Assignment Solution

### Project: Global Sales Dashboard

Build a professional one-page Power BI report using the Global Superstore 2022 dataset. The assignment requires four core visuals, interactive filters, branding, and publishing to Power BI Service.

---

## 1. Dashboard Visuals

### A. Sales by Region – Map

**Visual:** Map / Azure Maps (depending on Power BI availability)

- Location: `Region`
- Size/Values: `Sales`
- Tooltips: `Profit`, `Quantity`, `Discount`
- Title: **Sales by Region**

If geographic mapping does not work reliably for the Region field, use a filled map with a valid geographic column such as State/City/Country, or use a bar chart as a fallback.

### B. Category-wise Profit – Bar Chart

**Visual:** Clustered bar chart

- Y-axis: `Category`
- X-axis: `Profit`
- Sort: Profit descending
- Data labels: On
- Title: **Profit by Category**

Recommended measure:

```DAX
Total Profit = SUM(Sales[Profit])
```

### C. Monthly Revenue Trend – Line Chart

**Visual:** Line chart

- X-axis: `Calendar[Month Year]`
- Y-axis: `Total Sales`
- Title: **Monthly Revenue Trend**
- Enable drill-down if a date hierarchy is used.

Recommended measure:

```DAX
Total Sales = SUM(Sales[Sales])
```

### D. Top 5 Customers – Table

**Visual:** Table

Fields:
- Customer
- Sales
- Profit
- Quantity

Apply a **Top N = 5** filter based on `[Total Sales]`.

---

## 2. Required Filters

Add three slicers:

1. **Year** → `Calendar[Year]`
2. **Segment** → `Sales[Segment]`
3. **Region** → `Sales[Region]`

All visuals should respond to the slicers. Use **Format → Edit interactions** to verify that the slicers filter the intended visuals.

---

## 3. KPI Cards

The project task requires KPIs for Sales, Profit, and Discount.

### Sales KPI
```DAX
Total Sales = SUM(Sales[Sales])
```

### Profit KPI
```DAX
Total Profit = SUM(Sales[Profit])
```

### Discount KPI
```DAX
Total Discount = SUM(Sales[Discount])
```

Add three Card visuals:
- **Total Sales**
- **Total Profit**
- **Total Discount**

Format sales/profit as currency and discount consistently with the dataset's units.

---

## 4. Highlight Top 5 Products and Regions

### Top 5 Products

Use a bar chart or table with:
- Product on Axis/Rows
- `[Total Sales]` or `[Total Profit]` as Values
- Visual-level filter → **Top N → 5**

Recommended: rank by `[Total Sales]`.

### Top Regions

Use a region visual sorted by `[Total Sales]` descending. For a Top N version:

- Region → Filters
- Filter type → Top N
- Top → 5
- By value → `[Total Sales]`

---

## 5. Calendar Table

Use a dedicated calendar table for reliable monthly analysis:

```DAX
Calendar =
CALENDAR(
    MIN(Sales[Order Date]),
    MAX(Sales[Order Date])
)
```

Add:

```DAX
Year = YEAR(Calendar[Date])

Month Number = MONTH(Calendar[Date])

Month = FORMAT(Calendar[Date], "MMMM")

Month Year = FORMAT(Calendar[Date], "MMM YYYY")
```

Sort `Month` and `Month Year` using `Month Number` (and a Year-Month sort column where necessary), then relate:

`Calendar[Date]` → `Sales[Order Date]`

---

## 6. Dashboard Layout

Recommended professional layout:

```text
+----------------------------------------------------------------+
|                 GLOBAL SALES DASHBOARD                          |
|                 [Company Logo]                                  |
+----------------+----------------+----------------+--------------+
| Total Sales    | Total Profit   | Total Discount | Year         |
+----------------+----------------+----------------+--------------+
| Region Slicer  | Segment Slicer | Region Slicer                 |
+----------------------------------------------------------------+
| Sales by Region (Map)        | Profit by Category (Bar)        |
|                              |                                  |
+----------------------------------------------------------------+
| Monthly Revenue Trend (Line) | Top 5 Customers (Table)         |
|                              |                                  |
+----------------------------------------------------------------+
```

Keep spacing, alignment, font sizes, titles, and number formats consistent.

---

## 7. Theme & Branding

A theme file is included in this folder: `global-sales-theme.json`.

To apply it:

1. Open Power BI Desktop.
2. Select **View → Themes → Browse for themes**.
3. Select `global-sales-theme.json`.
4. Add the company logo using an Image visual.
5. Keep the logo in the header area and avoid covering report content.

Recommended design principles:
- One primary brand color.
- One secondary accent color.
- Neutral background.
- Consistent typography.
- Strong contrast for KPI values.
- Minimal decorative elements.

---

## 8. Interactivity

### Drill-through

Create a dedicated **Customer Detail** page:
- Add `Customer` to the Drill-through field.
- Add Sales, Profit, Quantity, and Order Date visuals.
- Right-click a customer in the main report → **Drill through → Customer Detail**.

### Bookmarks

Create two useful bookmarks:

**Overview**
- Shows the standard dashboard view.

**Top Performers**
- Shows top products/customers/regions.

Steps:
1. Open **View → Bookmarks**.
2. Configure the desired visual/filter state.
3. Add a bookmark.
4. Add buttons and set **Action → Bookmark**.

---

## 9. Publish to Power BI Service

1. Save the `.pbix` file.
2. Sign in to Power BI Desktop using your organization/student account.
3. Select **Home → Publish**.
4. Choose the required workspace.
5. Wait for publishing to complete.
6. Open the report in Power BI Service.
7. Verify filters, visuals, bookmarks, and drill-through.
8. Use **Share** or the workspace/report sharing options according to your organization's permissions.

### Important
The actual Power BI Service share URL can only be generated after the `.pbix` report has been published to an authenticated Power BI workspace. It cannot be fabricated or generated from GitHub alone.

---

# Practice Set Answers

## Q1. Design a dashboard using 3 visuals

Use:
1. Sales by Region – Map
2. Profit by Category – Bar Chart
3. Monthly Revenue – Line Chart

Add a Year slicer for interactivity and place the visuals in a clean grid.

## Q2. Add logo & theme

- Import `global-sales-theme.json` from **View → Themes → Browse for themes**.
- Insert the company logo with **Insert → Image**.
- Place it in the dashboard header.
- Keep the same theme across all report pages.

## Q3. Publish report to Power BI Service

- Save the `.pbix` report.
- Sign in to Power BI.
- Select **Home → Publish**.
- Select the correct workspace.
- Open the published report in Power BI Service.
- Test all interactions before sharing.

## Q4. Share dashboard link

After publishing, select **Share** and copy the generated report/share link if your organization's Power BI permissions allow sharing. The link depends on the authenticated Power BI workspace and therefore must be generated from the user's Power BI Service account.

---

# Final Submission Checklist

- [x] 4 required visuals specified
- [x] Sales by Region map
- [x] Category-wise Profit bar chart
- [x] Monthly Revenue Trend line chart
- [x] Top 5 Customers table
- [x] Year filter
- [x] Segment filter
- [x] Region filter
- [x] Sales KPI
- [x] Profit KPI
- [x] Discount KPI
- [x] Top 5 products
- [x] Top regions
- [x] Calendar table and date hierarchy
- [x] Theme and branding plan
- [x] Drill-through
- [x] Bookmarks
- [x] Power BI Service publishing steps
- [x] Sharing instructions
