# Week 1 – Power BI Basics & Data Modelling

This folder contains the completed Week 1 Power BI assignment and practice-set solution.

## Practice Questions

1. **Import Excel dataset into Power BI**
   - Open Power BI Desktop → **Home → Get Data → Excel** (or CSV).
   - Select `week1_sales_data.csv` and load the `Sales` table.

2. **Replace null values with `Not Available`**
   - Open **Home → Transform data** to launch Power Query.
   - Select the relevant text column → **Transform → Replace Values**.
   - Replace `null`/blank values with `Not Available`.
   - For a reusable Power Query approach, use:
     `Table.ReplaceValue(Source, null, "Not Available", Replacer.ReplaceValue, {"Customer"})`

3. **Create calculated column: Total = Qty × Price**
   - In Power BI: **Table tools → New column**.
   - DAX:
     `Total = Sales[Qty] * Sales[Price]`

4. **Rename and format data fields**
   - Rename `Qty` to `Quantity` and `Price` to `Unit Price` if desired.
   - Set `Quantity` to Whole Number.
   - Set `Unit Price`, `Total`, and `Profit` to Currency/Decimal as appropriate.
   - Format `Region` and `Product` as Text.

## Dashboard Project

Create a one-page sales dashboard containing:

- **Total Revenue** – `SUM(Sales[Total])`
- **Sales by Region** – clustered column/bar chart using Region and Total Revenue
- **Top 5 Products by Profit** – bar chart using Product and Total Profit, filtered to Top N = 5

### Recommended DAX measures

```DAX
Total Revenue = SUM(Sales[Total])

Total Profit = SUM(Sales[Profit])

Total Quantity = SUM(Sales[Qty])
```

### Top 5 Products

For the visual, place `Product` on the axis and `Total Profit` in Values. In the Filters pane:

- Filter type: **Top N**
- Show items: **Top 5**
- By value: **Total Profit**

## Expected Results for the Included Dataset

- **Total Revenue:** 65,750
- **Sales by Region:** West 32,570; North 18,360; East 9,930; South 4,890
- **Top 5 Products by Profit:** Laptop, Printer, Tablet, Webcam, Headset

The supplied PDF assignment asks for a simple sales dashboard and the practice set covers importing, Power Query cleaning, calculated columns, and field formatting. The solution above implements all requested items using the included sample dataset.
