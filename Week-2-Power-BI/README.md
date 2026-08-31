# Week 2 – Data Modeling & Visualization

## Assignment: Data Modeling & Relationships

### 1. Create Relationships

Use **Model view** in Power BI and create these relationships:

| From | Key | To | Key | Cardinality |
|---|---|---|---|---|
| Orders | Customer ID | Customers | Customer ID | Many-to-One (*:1) |
| Orders | Product ID | Products | Product ID | Many-to-One (*:1) |
| Orders | Region ID | Regions | Region ID | Many-to-One (*:1) |

Recommended relationship settings:
- **Cardinality:** Many-to-one (*:1)
- **Cross-filter direction:** Single, from dimension table to Orders
- Ensure each dimension key is unique.
- Avoid unnecessary many-to-many relationships.

### 2. Create Calendar Table

Create a dedicated date table with DAX:

```DAX
Calendar =
CALENDAR(
    MIN(Orders[Order Date]),
    MAX(Orders[Order Date])
)
```

Add useful date columns:

```DAX
Year = YEAR(Calendar[Date])

Month Number = MONTH(Calendar[Date])

Month = FORMAT(Calendar[Date], "MMMM")

Month Year = FORMAT(Calendar[Date], "MMM YYYY")

Day = DAY(Calendar[Date])
```

For correct month sorting:
1. Select `Calendar[Month]`.
2. Choose **Column tools → Sort by column → Month Number**.

Then mark it as a date table:
**Table tools → Mark as date table → Date**.

Create the relationship:
`Calendar[Date]` → `Orders[Order Date]` as **One-to-Many (1:*)**.

### 3. Date Hierarchy

Build the hierarchy:

**Year → Month → Day**

Recommended hierarchy columns:
- Year
- Month Number / Month
- Day

Use the hierarchy on charts so users can drill down from yearly results to monthly and daily results.

### 4. Star Schema

The preferred model is a star schema:

```text
             Customers
                 |
                 |
Products ---- Orders ---- Regions
                 |
                 |
              Calendar
```

- `Orders` is the central **fact table**.
- `Customers`, `Products`, `Regions`, and `Calendar` are **dimension tables**.
- Relationships should generally flow from dimensions to the fact table.

A snowflake schema can further normalize dimensions, but the star schema is simpler and generally preferable for Power BI reporting.

### 5. Data Validation Checklist

Before creating visuals, verify:

- Customer IDs in `Customers` are unique.
- Product IDs in `Products` are unique.
- Region IDs in `Regions` are unique.
- Every Orders foreign key matches a dimension key.
- Order Date values fall within the Calendar table range.
- No unexpected blank keys exist.
- Relationship cardinalities are correct.
- Model View shows all required relationships.
- Revenue totals are consistent before and after modeling.

---

# Week 2 – Visualization Practice Set

## 1. Region-wise Sales Bar Chart

Create a bar/column chart:
- **Axis:** `Region`
- **Values:** `Sales` / `Total Revenue`
- Sort by sales in descending order.
- Add a descriptive title such as **Sales by Region**.

Example measure:

```DAX
Total Sales = SUM(Orders[Sales])
```

## 2. Year Slicer

Add a slicer:
- Field: `Calendar[Year]`
- Use Dropdown or List style.
- The slicer should filter all connected visuals on the report page.

## 3. Interactive Revenue Trend Line Chart

Create a line chart:
- **X-axis:** `Calendar[Date]` or `Calendar[Month Year]`
- **Y-axis:** `Total Sales`
- Turn on drill-down when using the date hierarchy.
- Add the Year slicer to make the trend interactive.

Recommended measure:

```DAX
Revenue = SUM(Orders[Sales])
```

## 4. Visual Formatting

Apply these formatting practices:
- Give every visual a clear, meaningful title.
- Use legends only where they add information.
- Format currency measures consistently.
- Display data labels when they improve readability.
- Keep fonts and sizing consistent.
- Use appropriate axis titles.
- Avoid unnecessary borders and visual clutter.
- Ensure slicers and charts are aligned neatly.

## Final Dashboard Layout

A strong Week 2 report page can contain:

1. **Year Slicer** at the top.
2. **Sales by Region** bar chart on the left.
3. **Revenue Trend** line chart on the right.
4. Optional KPI card for **Total Sales**.

This completes the Week 2 requirements for data modeling, relationships, calendar-table creation, date hierarchy, validation, and interactive visualization.
