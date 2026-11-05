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

