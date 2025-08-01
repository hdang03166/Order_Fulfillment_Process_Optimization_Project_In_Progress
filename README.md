# Order Fulfillment Process Optimization Project
## Note: I am currently working on this self-initiated project. I have completed the cleaning and EDA phases and am about to start building the Power BI dashboard. I will post the completed version on LinkedIn soon—stay tuned! (Updated July 2nd)

## Overview
This project analyzes how orders move through an e-commerce system, from customer order placement to delivery. The end-to-end analysis discovers trends, tracks key performance indicators, and suggests improvements. Data is explored using Python and cleaned with Excel, applying ETL processes and exploratory data analysis. Advanced SQL queries extract insights that are visualized in a Power BI dashboard with visuals, DAX calculations, and basic forecasting to deliver business insights.

## Objective
Identify problems in the order and delivery process, forecast key metrics such as order volume, and provide actionable insights to optimize operations and enhance customer experience.

## Data Source
The dataset used in this project is publicly available on Kaggle:
[Ecommerce Order & Supply Chain Dataset by Aditya Bagus Pratama](https://www.kaggle.com/datasets/bytadit/ecommerce-order-dataset/data)

It covers the fulfillment pipeline from order placement through delivery, including order-level data, shipping and delivery timestamps, customer and product details, shipping status, and more. Only the train folder was used for this project to simplify analysis.

License: Provided under Kaggle Terms of Use; all rights belong to the original author.


## Content Description
The train folder contains five CSV files that represent key entities involved in the e-commerce order fulfillment pipeline:

- df_Customers.csv: Contains customer identifiers and regional information.

- df_OrderItems.csv: Details the individual products within each order, including seller ID, price, and shipping charges.

- df_Orders.csv: Contains data such as order ID, order status, and shipping and delivery dates.

- df_Payments.csv: Logs payment information per order, including payment type, installments, and amount.

- df_Products.csv: Lists product data such as category name and physical measurements.



## Data Cleaning & Preparation
General Data Formatting Updates
- Standardized all ID columns (`customer_id`, `product_id`, `order_id`, `seller_id`) to uppercase across all datasets for readability and consistency.

- Formatted monetary columns in `orderitems` and `payments` to display two decimal places for clarity.

- Cleaned product category names for spelling and grammar consistency.

- Converted helper columns to static values to improve Excel performance with large datasets (~89,000 rows).

<br>

Exploratory Data Analysis of Cleaned Orders Sheet
- Added 5 helper columns (H to L) to identify data quality issues and assist in KPI calculations:

  - order_status_timestamp_issue (Column H): Flags rows where `order_status` = "delivered" but `order_delivered_timestamp` is missing: =IF(AND($C2="delivered", ISBLANK($F2)), "Missing Delivered Timestamp", "OK")

  - count_timestamp_issue (Column I): Counts total rows with delivery timestamp issues flagged in Column H: =COUNTIF(H2:H89317, "Missing Delivered Timestamp")

  - order_approved_flag (Column J): Flags missing order_approved_at timestamps: =IF(ISBLANK(E2), "Missing", "Present")

  - missing_order_approved (Column K): Counts total missing approval timestamps flagged in Column J: =COUNTIF(J2:J89317, "Missing")

  - missing_delivered_timestamp (Column L): Counts total missing delivery timestamps: =COUNTIF(F2:F89317, "")

- Applied conditional formatting with orange fill and solid outline border for clear visibility on key columns:

  - Highlighted cells in Column F (`order_delivered_timestamp`) where `order_status` = "delivered" but the timestamp is blank using(=AND($C2="delivered", ISBLANK($F2)).

  - Highlighted all blank cells in Column F (`order_delivered_timestamp`) using =ISBLANK($F2).

  - Highlighted cells in Column E (`order_approved_at`) where the helper column J (`order_approved_flag`) marks the value as "Missing" using =$J2="Missing".

<br>

Exploratory Data Analysis of Cleaned Products Sheet
- Added 5 helper columns (G to K) to identify data quality issues and assist in KPI calculations:

  - concatenation_key (Column G): Combined multiple columns into a single key to detect exact duplicate rows: =A2 & "|" & B2 & "|" & C2 & "|" & D2 & "|" & E2 & "|" & F2

  - duplicate_label (Column H): Flags rows as "Duplicate" or "Unique" based on the concatenation key: =IF(COUNTIF($G$2:G2, G2) > 1, "Duplicate", "Unique")

  - total_duplicates Count (Column I): Counts how many duplicate rows exist in the dataset: =COUNTIF(H2:H89304, "Duplicate")

  - missing_values Count (Column J): Counts the number of missing values across key product columns for each row: =COUNTBLANK(A2:F89304)

  - data_quality_flag (Column K): Marks rows with critical missing data or inconsistencies for easier filtering.

- Deleted 13 exact duplicate rows to avoid skewing KPIs and analyses.

- Standardized and corrected product category names for consistency and accuracy, including:

  - “perfumery” → “perfumes”

  - “telephony” → “telephones”

  - “home_confort” → “home_comfort”

  - “fixed_telephony” → “fixed_telephones”

  - “fashio_female_clothing” → “fashion_female_clothing”

- Applied conditional formatting with orange fill and solid outline border for clear visibility on key columns:

  - Highlighted cells in Column B (`product_category_name`) using =ISBLANK(B2).

  - Highlighted cells in Column C to F using =ISBLANK(C2).


## Tools & Techniques
- **Kaggle Notebook**: Utilized Python to load files, preview data, and identify missing values and duplicates.
- **SQL Server Express & SSMS**: Wrote advanced SQL queries, demonstrating core project work and KPIs.
- **Microsoft Excel**: Showcased ETL processes and validation, using Power Query, formulas (e.g., COUNTBLANK, IF), and conditional formatting.
- **Power BI**: Created dashboard visualization and DAX calculations for metrics and KPIs.

---
# Note to self: The sections above have been updated; the sections below still need modification.
---

## Project Structure
```
Order_Fulfillment_Process_Optimization_Project/
├── 1_data/
│   ├── raw_data_unchanged.xlsx
│   ├── raw_and_cleaned_data.xlsx
│   └── original_cvs_folder/
│       ├── df_customers.csv
│       ├── df_orderItems.csv
│       ├── df_orders.csv
│       ├── df_payments.csv
│       └── df_products.csv
├── 2_analysis/
│   └── order_fulfillment_process_optimization.ipynb
├── 3_powerbi/
│   └── order_fulfillment_dashboard.pbix
├── 4_visuals/
│   └── dashboard_visual.png
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## Key Questions & Insights  
- What are the average and distribution of fulfillment times across different shipping methods?  
- Which stages in the process (order processing, shipping, last-mile delivery) contribute most to delays?  
- How do product types or regions affect fulfillment efficiency?  
- Forecast trends in order volume and fulfillment times to predict peak periods and capacity needs.  
- Recommendations for process improvements based on data-driven insights.

## Power BI Dashboard  
An interactive Power BI dashboard visualizes fulfillment KPIs, bottlenecks, and forecasted trends. It leverages DAX for dynamic calculations such as average delay per stage, percentage of on-time deliveries, and rolling forecast metrics.

Dashboard features:
- Drill-down by shipping method, product category, and region.
- Time series analysis of fulfillment times and delays.
- Forecast visuals for upcoming order volume and resource planning.

## How to Use This Repository
- Explore raw data under `/1_data/`.
- Explore data cleaning and forecasting in Jupyter Notebook under `/2_analysis/`.
- Open the Power BI dashboard file in `/3_powerbi/` using Power BI Desktop.
- View the dashboard screen under `/4_visuals/` for a quick visual reference.
- Read this README for project context, methodology and key insights.

## Contact
- [LinkedIn](https://www.linkedin.com/in/hai-dang316)
- Email: h.dang686@yahoo.com

Note: This project showcases skills in end-to-end data analysis, process improvement, forecasting, and dashboard visualization combining Python and Power BI.
