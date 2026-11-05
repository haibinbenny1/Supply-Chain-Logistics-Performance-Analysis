# Supply-Chain-Logistics-Performance-Analysis

# Excel Data Cleaning and Preparation

## 1. Project Overview

Used Microsoft Excel Power Query Editor to clean and prepare a Supply Chain and Logistics dataset for Power BI analysis.

The main tasks included removing unnecessary columns, correcting data types, checking data quality, validating dates, and creating new columns to analyse shipping performance.

## 2. Tools Used

- Microsoft Excel
- Power Query Editor

## 3. Data Cleaning Steps

### 3.1 Data Import and Inspection
- Imported the CSV dataset into Power Query Editor.
- Checked column names, data types, missing values, and errors.
- Used Column Quality, Column Distribution, and Column Profile.

### 3.2 Removing Unnecessary Columns
Removed 8 columns that were not required for the analysis:

- `latitude`
- `longitude`
- `customer_zipcode`
- `department_id`
- `category_id`
- `product_category_id`
- `order_item_cardprod_id`
- `product_card_id`

Rearranged the remaining 33 columns into logical groups.

### 3.3 Correcting Data Types
- Converted `order_date` and `shipping_date` to Date.
- Converted important ID columns to Text.
- Assigned suitable numeric data types to price, sales, quantity, and discount columns.
- Set categorical columns to Text.

### 3.4 Data Quality Checks
- Checked missing values and errors.
- Validated product prices, sales, quantities, and discount rates.
- Checked for negative and zero values.
- Kept negative profit values because they can represent genuine business losses.

No records matching the numerical validation conditions were found.

### 3.5 Duplicate and ID Validation
- Checked repeated `order_item_id` values.
- Found 6 distinct IDs with repeated occurrences.
- Grouped records using `order_id` and `order_item_id`.
- Found one repeated combination: `order_id = 1` and `order_item_id = 1`.
- Inspected the records and found different order statuses.

Kept the original records without deleting any data.

### 3.6 Date Validation
Created a custom column called `Date Validation` to identify records where the shipping date was earlier than the order date.

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

### 3.7 Shipping Performance Columns

Created three custom columns:

**Shipping Days**

Calculates the number of days between the order date and shipping date.

```powerquery
Duration.Days([shipping_date] - [order_date])
```

**Clean Shipping Days**

Replaces negative shipping durations with null values.

```powerquery
if [Shipping Days] < 0 then null
else [Shipping Days]
```

**Shipping Performance**

Classifies orders based on shipping duration.

```powerquery
if [Clean Shipping Days] = null then "Invalid Date"
else if [Clean Shipping Days] <= 7 then "Fast Delivery"
else if [Clean Shipping Days] <= 30 then "Normal Delivery"
else "Delayed Delivery"
```

The 7-day and 30-day limits were selected for this project.

### 3.8 Categorical Analysis
Used Reference queries, Group By, and Count Rows to check the distribution of:

- Order Status
- Payment Type
- Shipping Mode
- Market
- Customer Segment
- Product Category
- Department
- Shipping Performance

## 4. Final Dataset Summary

| Description | Result |
|---|---:|
| Total Records | 15,549 |
| Original Columns Retained | 33 |
| Columns Removed | 8 |
| New Columns Created | 4 |
| Final Columns | 37 |

**New columns created:**
1. `Date Validation`
2. `Shipping Days`
3. `Clean Shipping Days`
4. `Shipping Performance`

All 15,549 original records were retained.

