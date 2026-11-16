# Supply Chain & Logistics Performance Analysis

## 1. Project Overview

This project focuses on analysing supply chain and logistics performance using Excel, Power Query and Power BI.

The main objective was to clean and prepare the dataset, analyse sales and delivery performance, and build interactive dashboards to understand business performance and identify areas that need improvement.

## 2. Tools Used

- Microsoft Excel
- Power Query Editor
- Power BI
- DAX (Data Analysis Expressions)

## 3. Dataset Information

- Dataset: Supply Chain & Logistics Performance
- Total Records: 15,549
- Original Columns: 37
- Columns Removed: 5
- Original Columns Retained: 32
- New Columns Created: 4
- Final Columns: 36

# Part 1: Excel Data Cleaning

## 4. Data Import

Imported the original dataset into Excel Power Query Editor for data cleaning and transformation.

## 5. Removing Unnecessary Columns

Removed the following columns because they were not required for the analysis:

- latitude
- longitude
- product_card_id
- category_id
- department_id

## 6. Data Type Correction

Reviewed and corrected the data types of the columns.

- Converted order_date and shipping_date into Date format.
- Checked numerical columns such as price, sales, quantity and discount.
- Converted ID columns into Text format where appropriate.

## 7. Duplicate Analysis

Performed duplicate analysis using order_id and order_item_id.

Identified repeated records and reviewed their order statuses. The records were retained because they represented different order statuses rather than confirmed duplicate transactions.

## 8. Data Transformation

Created four new columns to support the analysis:

1. **Date Validation** – Identified valid and invalid date records.
2. **Shipping Days** – Calculated the number of days between order date and shipping date.
3. **Clean Shipping Days** – Replaced negative shipping durations with null values.
4. **Shipping Performance** – Classified delivery records based on shipping duration.

## 9. Shipping Performance Classification

Used the following conditions to classify shipping performance:

- Fast Delivery: Up to 7 days
- Normal Delivery: 8–30 days
- Delayed Delivery: More than 30 days
- Invalid Date: Records with invalid dates

These thresholds were defined for this project to support delivery performance analysis.

## 10. Data Profiling and Reference Queries

Used data profiling and Group By operations to understand the dataset.

Created categorical reference queries for:

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

## 11. Final Dataset Summary

| Description | Result |
|---|---:|
| Total Records | 15,549 |
| Original Columns | 37 |
| Columns Removed | 5 |
| Original Columns Retained | 32 |
| New Columns Created | 4 |
| Final Columns | 36 |

### Date Validation Results

| Status | Records |
|---|---:|
| Valid | 9,608 |
| Invalid | 5,941 |

### Shipping Performance Results

| Category | Records |
|---|---:|
| Fast Delivery | 1,167 |
| Normal Delivery | 2,643 |
| Delayed Delivery | 5,798 |
| Invalid Date | 5,941 |

# Part 2: Power BI Analysis

## 12. Data Import and Data Modelling

Imported the cleaned Excel dataset into Power BI.

Created a separate Date Table using DAX for date-based analysis.

Connected the Date Table to the main dataset using the order_date column.

Configured the relationship as:
- One-to-many relationship
- Single cross-filter direction
- Active relationship

Marked the Date Table as the official Date Table in Power BI.

## 13. Calculated Columns

Created the following calculated columns using DAX:

- Category Type
- Revenue per Order
- Sales Category

These columns were used to support category analysis and sales comparisons.

## 14. DAX Measures

Created measures to calculate important business metrics:

- Order Count
- Total Orders
- Total Sales
- Total Profit
- Total Quantity
- Average Shipping Days
- Profit Margin %
- Total Discount
- Average Profit in Delhi
- YTD Sales
- Fast Delivery Records
- Delayed Delivery Records
- Invalid Shipping Records

## 15. Dashboard 1 – Executive Overview

Created the Executive Overview dashboard to provide a summary of overall business performance.

### KPI Cards

- Total Sales
- Total Profit
- Total Orders
- Profit Margin

### Visualizations

1. Monthly Sales Trend – Line Chart
2. Profit by Market – Donut Chart
3. Sales by Product Category – Clustered Bar Chart
4. Shipping Performance Analysis – Clustered Column Chart
5. Sales by Customer Segment – Clustered Bar Chart
6. Sales by Shipping Mode – Clustered Column Chart

### Slicers

- Date
- Shipping Mode
- Customer Segment
- Market

## 16. Dashboard 2 – Shipping & Logistics Analysis

Created a separate dashboard focused on shipping duration and delivery performance.

### KPI Cards

- Average Shipping Days
- Fast Delivery Records
- Delayed Delivery Records
- Invalid Shipping Records

### Visualizations

1. **Shipping Mode Performance Comparison** – Line and Clustered Column Chart
2. **Shipping Performance Breakdown** – Donut Chart
3. **Shipping Performance by Department** – Ribbon Chart
4. **Shipping Performance by Market** – Stacked Column Chart
5. **Average Shipping Days by Market** – Clustered Bar Chart
6. **Shipping Performance by Shipping Mode** – 100% Stacked Bar Chart

The dashboard helps compare shipping performance across different markets, departments and shipping modes.

## 17. Dashboard Design

Used a consistent design across both dashboard pages.

- Applied a blue background theme.
- Used separate colour combinations for KPI cards and charts.
- Added clear chart titles.
- Arranged visuals to make the dashboards easy to understand.

## 18. Final Project Outcome

Completed the data cleaning, transformation, modelling and visualization process using Excel, Power Query and Power BI.

The project contains two interactive dashboard pages:

1. Executive Overview
2. Shipping & Logistics Analysis

This project helped me gain practical experience in data cleaning, data transformation, data modelling, DAX calculations and dashboard development using a supply chain and logistics dataset.
