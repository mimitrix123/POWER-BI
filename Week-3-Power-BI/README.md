# Week 3 – DAX Functions & KPI Visuals

## Assignment Solution

### 1. DAX Measures

Create these measures in the `Sales` table.

```DAX
Total Sales = SUM(Sales[Sales])

Total Profit = SUM(Sales[Profit])

Profit Margin = DIVIDE([Total Profit], [Total Sales], 0)
```

`DIVIDE()` is preferred over `/` because it safely handles a zero denominator.

### Useful additional measures

```DAX
Average Sales = AVERAGE(Sales[Sales])

Total Quantity = SUM(Sales[Quantity])

Total Discount = SUM(Sales[Discount])

Average Profit = AVERAGE(Sales[Profit])
```

### SUMX example

When a row-level calculation is required, use `SUMX()`:

```DAX
Calculated Sales =
SUMX(
    Sales,
    Sales[Quantity] * Sales[Sales]
)
```

Use the actual unit-price column instead if the dataset stores unit price rather than line sales.

### CALCULATE example

```DAX
West Sales =
CALCULATE(
    [Total Sales],
    Sales[Region] = "West"
)
```

Category filter example:

```DAX
Technology Sales =
CALCULATE(
    [Total Sales],
    Sales[Category] = "Technology"
)
```

---

## 2. Three KPI Cards

Create three Card visuals:

### Card 1 – Total Sales
- Visual: **Card**
- Field: `[Total Sales]`
- Title: **Total Sales**
- Format: Currency

### Card 2 – Total Profit
- Visual: **Card**
- Field: `[Total Profit]`
- Title: **Total Profit**
- Format: Currency

### Card 3 – Profit Margin
- Visual: **Card**
- Field: `[Profit Margin]`
- Title: **Profit Margin**
- Format: Percentage, preferably 1–2 decimal places

---

## 3. Region and Year Slicers

### Region slicer
1. Insert a **Slicer** visual.
2. Add `Sales[Region]`.
3. Use Dropdown/List as appropriate.

### Year slicer
If a Calendar table from Week 2 is available:
1. Add `Calendar[Year]` to a Slicer.
2. Confirm the Calendar → Sales relationship is active.

If no Calendar table is available, create one:

```DAX
Calendar = CALENDAR(MIN(Sales[Order Date]), MAX(Sales[Order Date]))
```

Then add:

```DAX
Year = YEAR(Calendar[Date])
```

Relate `Calendar[Date]` to `Sales[Order Date]`.

---

## 4. Conditional Formatting – Negative Profit

For a table/matrix containing Profit:

1. Select the visual.
2. Open the dropdown for the Profit field.
3. Choose **Conditional formatting → Font color** or **Background color**.
4. Set a rule:
   - If Profit `< 0` → **Red**.
   - If Profit `>= 0` → normal/positive formatting.
5. Apply the rule and verify negative-profit rows are clearly highlighted.

---

# Project Task – Retail Store KPI Dashboard

## Recommended dashboard layout

```text
+------------------------------------------------+
|             RETAIL STORE KPI DASHBOARD         |
+----------------+----------------+--------------+
|  Total Sales   |  Total Profit  | Profit Margin|
+----------------+----------------+--------------+
| Region Slicer  | Category Slicer | Year Slicer  |
+------------------------------------------------+
|                                                |
|       Sales by Region / Category (Bar)         |
|                                                |
+------------------------------------------------+
|                                                |
|           Sales & Profit Trend (Line)          |
|                                                |
+------------------------------------------------+
```

### Required filters

Add slicers for:
- Region
- Category
- Year

All KPI cards and charts should respond to these filters.

### Bar chart
Create a bar/column chart:
- Axis: `Region` or `Category`
- Values: `[Total Sales]`
- Sort descending by Total Sales
- Title: **Sales by Region** or **Sales by Category**

### Line chart
Create a line chart:
- X-axis: `Calendar[Month Year]` (preferred)
- Y-axis: `[Total Sales]`
- Optional second value: `[Total Profit]`
- Title: **Sales & Profit Trend**

### KPI insight examples
Use the dashboard to identify:
- Which region generates the highest sales.
- Which region/category generates the highest profit.
- Whether profit margin improves or declines over time.
- Which filtered period has the strongest sales performance.
- Where negative-profit transactions require attention.

---

# Practice Set Solutions

## Q1. DAX for Total Sales by Category

A measure automatically calculates total sales in the current category filter context:

```DAX
Total Sales = SUM(Sales[Sales])
```

Place `Category` on a visual axis and `[Total Sales]` in Values to obtain category-wise totals.

An explicit category calculation can be written as:

```DAX
Category Sales =
CALCULATE(
    [Total Sales],
    VALUES(Sales[Category])
)
```

The first version is simpler and recommended for normal visuals.

## Q2. Calculate Profit Margin

```DAX
Profit Margin =
DIVIDE(
    [Total Profit],
    [Total Sales],
    0
)
```

Format the measure as a percentage.

## Q3. Use CALCULATE() with Filters

```DAX
East Region Sales =
CALCULATE(
    [Total Sales],
    Sales[Region] = "East"
)
```

Multiple filters can also be applied:

```DAX
East Technology Sales =
CALCULATE(
    [Total Sales],
    Sales[Region] = "East",
    Sales[Category] = "Technology"
)
```

## Q4. Build KPI Showing Sales vs Target

First create a target measure. Example:

```DAX
Sales Target = 100000
```

Then create:

```DAX
Sales vs Target = [Total Sales] - [Sales Target]

Sales Achievement % =
DIVIDE([Total Sales], [Sales Target], 0)
```

For a more useful KPI, create a KPI visual:
- **Indicator:** `[Total Sales]`
- **Target:** `[Sales Target]`
- **Trend axis:** `Calendar[Date]` or `Calendar[Month Year]`

If the target should change by year/month, store targets in a separate Target table and relate it to the appropriate date dimension instead of hard-coding one number.

---

# Final Checklist

- [x] Total Sales measure
- [x] Total Profit measure
- [x] Profit Margin measure using DIVIDE
- [x] SUMX example
- [x] AVERAGE measures
- [x] CALCULATE with filters
- [x] Three KPI cards
- [x] Region slicer
- [x] Year slicer
- [x] Category filter
- [x] Negative-profit conditional formatting
- [x] Sales bar chart
- [x] Sales/profit trend line chart
- [x] Sales-vs-target KPI
- [x] Dashboard insights

**Dataset columns specified in the Week 3 assignment:** Order Date, Region, Category, Sales, Profit, Quantity, Discount.
