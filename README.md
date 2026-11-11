# Supply-Chain-Logistics-Performance-Analysis

# Excel Data Cleaning and Preparation

## 1. Project Overview

Used Microsoft Excel Power Query Editor to clean and prepare a Supply Chain and Logistics dataset before importing it into Power BI.

The main tasks included removing unnecessary columns, checking data quality, correcting data types, investigating duplicates, validating shipping dates, and creating new columns to analyse delivery performance.

## 2. Tools Used

- Microsoft Excel
- Power Query Editor

## 3. Data Import and Inspection

- Imported the original CSV file into Power Query Editor.
- Checked the dataset structure and column names.
- Used Column Quality, Column Distribution, and Column Profile to inspect missing values, errors, and distinct values.

## 4. Removing Unnecessary Columns

Removed 5 columns that were not required for the analysis:

1. `latitude`
2. `longitude`
3. `product_card_id`
4. `category_id`
5. `department_id`

Rearranged the remaining columns into logical groups for easier analysis.

## 5. Data Type Correction

- Converted `order_date` and `shipping_date` to Date.
- Checked numerical columns such as price, sales, quantity, and discount.
- Reviewed categorical columns and their data types.
- Checked ID columns for consistency.

## 6. Data Quality Checks

Checked the dataset for missing values, errors, and unusual numerical values.

The following columns were validated:

- `product_price`
- `order_item_product_price`
- `sales`
- `order_item_quantity`
- `order_item_discount_rate`

Checked for negative and zero prices, sales, and quantities, along with discount rates outside the 0–1 range.

No records matching these validation conditions were found.

Negative profit values were retained because they can represent genuine business losses.

## 7. Duplicate and ID Validation

- Checked repeated values in `order_item_id`.
- Found 6 distinct IDs with repeated occurrences.
- Grouped records using `order_id` and `order_item_id`.
- Found one repeated combination: `order_id = 1` and `order_item_id = 1`.
- Inspected the records and found different order statuses.

Kept the original records instead of deleting them.

## 8. Date Validation

Created a custom column called `Date Validation` to identify records where the shipping date was earlier than the order date.

**Formula:**

```powerquery
if [shipping_date] < [order_date]
then "invalid"
else "valid"
```

| Status | Records |
|---|---:|
| Valid | 9,608 |
| Invalid | 5,941 |
| Total | 15,549 |

Invalid date records were retained for further analysis.

## 9. Shipping Days Calculation

Created a custom column called `Shipping Days` to calculate the number of days between the order date and shipping date.

**Formula:**

```powerquery
Duration.Days([shipping_date] - [order_date])
```

## 10. Clean Shipping Days

Created another column called `Clean Shipping Days` to replace negative shipping durations with null values.

**Formula:**

```powerquery
if [Shipping Days] < 0 then null
else [Shipping Days]
```

The original `Shipping Days` column was preserved.

## 11. Shipping Performance Classification

Created a custom column called `Shipping Performance` to classify orders based on shipping duration.

**Formula:**

```powerquery
if [Clean Shipping Days] = null then "invalid Date"
else if [Clean Shipping Days] <= 7 then "Fast Delivery"
else if [Clean Shipping Days] <= 30 then "Normal Delivery"
else "Delayed Delivery"
```

| Shipping Category | Records |
|---|---:|
| Fast Delivery | 1,167 |
| Normal Delivery | 2,643 |
| Delayed Delivery | 5,798 |
| Invalid Date | 5,941 |
| Total | 15,549 |

The 7-day and 30-day limits were selected for this project.

## 12. Categorical Data Analysis

Created separate Reference queries in Power Query and used Group By and Count Rows to analyse:

- Label
- Order Status
- Payment Type
- Shipping Mode
- Market
- Customer Segment
- Product Category
- Department
- Date Validation
- Shipping Performance

This helped understand the distribution of orders and other categories in the dataset.

## 13. Final Dataset Summary

| Description | Result |
|---|---:|
| Total Records | 15,549 |
| Original Columns | 37 |
| Columns Removed | 5 |
| Original Columns Retained | 32 |
| New Calculated Columns | 4 |
| Final Columns | 36 |

**New columns created:**

1. `Date Validation`
2. `Shipping Days`
3. `Clean Shipping Days`
4. `Shipping Performance`

All 15,549 original records were retained.

## 14. Power BI – Data Import and Preparation

After completing the data cleaning in Excel, imported the final cleaned dataset into Power BI Desktop.

- Imported the cleaned Excel workbook.
- Selected the `incom2024_delay_example_dataset` table.
- Checked the columns and data types.
- Applied the changes using Close & Apply.
- Verified the imported data in Data View.

## 15. Date Table and Data Modeling

Created a separate Date Table using DAX for time-based analysis.

**DAX Formula:**

```dax
Date Table =
ADDCOLUMNS(
    CALENDAR(
        MIN('incom2024_delay_example_dataset'[order_date]),
        MAX('incom2024_delay_example_dataset'[order_date])
    ),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Year Month", FORMAT([Date], "YYYY-MM")
)
```

The Date Table includes:

- Date
- Year
- Month Number
- Month
- Quarter
- Year Month

Created a relationship between the Date Table and the main dataset.

| Setting | Value |
|---|---|
| First Table | Date Table |
| First Column | Date |
| Second Table | incom2024_delay_example_dataset |
| Second Column | order_date |
| Cardinality | One-to-Many (1:*) |
| Cross-filter Direction | Single |
| Relationship | Active |

Also marked the Date Table as a Date Table in Power BI.

## 16. Calculated Columns – DAX

Created three calculated columns for additional analysis.

### 16.1 Category Type

Combined product category and department name.

```dax
Category Type =
'incom2024_delay_example_dataset'[category_name]
    & " - "
    & 'incom2024_delay_example_dataset'[department_name]
```

### 16.2 Revenue per Order

Calculated revenue using product price and quantity.

```dax
Revenue per Order =
'incom2024_delay_example_dataset'[order_item_product_price]
    * 'incom2024_delay_example_dataset'[order_item_quantity]
```

### 16.3 Sales Category

Compared product prices with the overall average product price.

```dax
Sales Category =
IF(
    'incom2024_delay_example_dataset'[order_item_product_price]
        > CALCULATE(
            AVERAGE('incom2024_delay_example_dataset'[order_item_product_price]),
            ALL('incom2024_delay_example_dataset')
        ),
    "Above Average",
    "Below Average"
)
```

## 17. DAX Measures

Created the following measures to calculate important sales and logistics metrics.

### Order Count

```dax
Order Count =
COUNT('incom2024_delay_example_dataset'[order_id])
```

### Total Orders

```dax
Total Orders =
DISTINCTCOUNT('incom2024_delay_example_dataset'[order_id])
```

### Total Sales

```dax
Total Sales =
SUM('incom2024_delay_example_dataset'[sales])
```

### Total Profit

```dax
Total Profit =
SUM('incom2024_delay_example_dataset'[order_profit_per_order])
```

### Total Quantity

```dax
Total Quantity =
SUM('incom2024_delay_example_dataset'[order_item_quantity])
```

### Average Shipping Days

```dax
Average Shipping Days =
AVERAGE('incom2024_delay_example_dataset'[Clean Shipping Days])
```

### Profit Margin %

```dax
Profit Margin % =
DIVIDE([Total Profit], [Total Sales], 0)
```

Formatted this measure as a percentage in Power BI.

### Total Discount

```dax
Total Discount =
SUMX(
    'incom2024_delay_example_dataset',
    'incom2024_delay_example_dataset'[order_item_discount]
)
```

### Average Profit in Delhi

```dax
Average Profit in Delhi =
CALCULATE(
    AVERAGE('incom2024_delay_example_dataset'[order_profit_per_order]),
    'incom2024_delay_example_dataset'[order_city] = "Delhi"
)
```

### YTD Sales

```dax
YTD Sales =
TOTALYTD(
    SUM('incom2024_delay_example_dataset'[sales]),
    'Date Table'[Date]
)
```

## 18. Power BI Dashboard

Created a single-page dashboard named **Supply Chain & Logistics Performance Analysis**.

### 18.1 KPI Cards

Added four KPI cards to display the main metrics:

1. Total Sales
2. Total Profit
3. Total Orders
4. Profit Margin %

### 18.2 Data Visualizations

Created six charts to analyse different areas of the dataset.

| Chart | Visualization Type |
|---|---|
| Monthly Sales Trend | Line Chart |
| Profit by Market | Donut Chart |
| Sales by Product Category | Clustered Bar Chart |
| Shipping Performance Analysis | Clustered Column Chart |
| Sales by Customer Segment | Clustered Bar Chart |
| Sales by Shipping Mode | Clustered Column Chart |

### 18.3 Interactive Slicers

Added four slicers to filter the dashboard:

- Date
- Market
- Customer Segment
- Shipping Mode

These filters allow users to explore sales and logistics performance based on different dates, markets, customer groups and shipping methods.

### 18.4 Dashboard Design

- Created a single-page dashboard.
- Used a blue background with contrasting chart panels.
- Arranged four KPI cards at the top.
- Added six charts in a two-row layout.
- Added slicers in a separate filter panel.
- Used percentage formatting for Profit Margin.

## 19. Final Outcome

Completed the Excel data cleaning and Power BI analysis using the prepared supply chain dataset.

The final dashboard brings together sales, profit, customer and shipping information, allowing users to explore different areas of supply chain performance through charts and interactive filters.
