# Order Fulfillment Process Optimization Project
## Note: I am currently working on this self-initiated project. I have completed the cleaning and EDA phases and am about to start building the Power BI dashboard. I will post the completed version on LinkedIn soon—stay tuned! (Updated July 2nd)

<br>

## Overview
This project explores trends in the e-commerce order fulfillment process using a multi-table dataset. It focuses on analyzing order performance, payment methods, and delivery timelines to identify process inefficiencies and customer experience gaps. Python was used for data exploration and cleaning, Excel for organization, SQL for KPI analysis, and Power BI for interactive visualization.

<br>

## Objective
Understand what drives order performance and customer experience by analyzing trends across the fulfillment lifecycle, and explore forecasting techniques to support better operational planning and business decisions.

## Data Source
The dataset used in this project is publicly available on Kaggle:
[Ecommerce Order & Supply Chain Dataset by Aditya Bagus Pratama](https://www.kaggle.com/datasets/bytadit/ecommerce-order-dataset/data)

It covers the fulfillment pipeline from order placement through delivery, including order-level data, shipping and delivery timestamps, customer and product details, shipping status, and more.

License: Provided under Kaggle Terms of Use; all rights belong to the original author.

<br>

## Content Description
The source dataset includes two folders: train and test. This project uses only the train folder, which contains five CSV files:

- df_Customers: Contains customer identifiers and regional information.

- df_OrderItems: Details the individual products within each order, including seller ID, price, and shipping charges.

- df_Orders: Order data such as order ID, order status, and shipping and delivery dates.

- df_Payments: Payment information per order, including payment type, installments, and amount.

- df_Products: Product data including category name and physical measurements.

<br>

## Data Cleaning & Preparation
- Standardized all text-based columns to uppercase and to middle alignment all datasets for consistency and improved readability, except for `customer_city`, it is only capitalized on the first letter of each word.

- The numeric monetary columns are displayed with two decimal places for clarity, and along with the timestamp columns, they are all right aligned for easier comparison and scanning. Only the product measurement columns were kept as whole integers.

- `Product_category_name` was cleaned up for spelling and grammar consistency.

- Converted all helper columns to static values to improve Excel performance with large datasets (~89,000 rows).

<br>

### Exploratory data analysis on cleaned_df_orders

Applied conditional formatting to the following with orange fill and solid outline boarders all around on missing cells for clear visibility:

- Highlighted cells in Column E (`order_approved_at`) where the helper column J (`order_approved_flag`) marks the value as "MISSING" or "PRESENT" with the formula =$J2="Missing".

- Highlighted cells in Column F (`order_delivered_timestamp`) where `order_status` = "DELIVERED" but the timestamp is blank with the formula =AND($C2="DELIVERED", ISBLANK($F2)).

- Highlighted all blank cells in Column F (`order_delivered_timestamp`) with the formula =ISBLANK($F2). 

Added 5 helper columns to identify data quality issues and assist in KPI calculations:

- (Column H) order_status_timestamp_issue: Flags rows where `order_status` = "delivered" but `order_delivered_timestamp` is missing: =IF(AND($C2="delivered", ISBLANK($F2)), "Missing Delivered Timestamp", "OK").

- (Column I) count_timestamp_issue: Counts total rows with delivery timestamp issues flagged in Column H with the formula =COUNTIF(H2:H89317, "Missing Delivered Timestamp").

- (Column J) order_approved_flag: Flags missing order_approved_at timestamps: =IF(ISBLANK(E2), "MISSING", "PRESENT").

- (Column K) missing_order_approved: Counts total missing approval timestamps flagged in Column J: =COUNTIF(J2:J89317, "Missing").

- (Column L) missing_delivered_timestamp: Counts total missing delivery timestamps: =COUNTIF(F2:F89317, "").

<br>

### Exploratory data analysis on cleaned_df_products

- Identified 14 entries (13 duplicates) of the same product_id (ZX9HL81JFVR2) in the Toys category. These entries lacked product measurement data. The rows were kept to preserve integrity across datasets.

- Applied conditional formatting to the range $B$2:$F$89317 with the formula =ISBLANK(B2) to highlight blank cells. Blank cells are formatted with orange fill and solid outline borders for improved visibility and easier identification of missing data.

Added 5 helper columns to identify data quality issues and assist in KPI calculations:

- (Column G) concatenation_key: Combined columns A to F into a single key for column H to detect duplicate or unique rows with the formula =A2 & "|" & B2 & "|" & C2 & "|" & D2 & "|" & E2 & "|" & F2.

- (Column H) duplicate_label: Flags rows as "DUPLICATE" or "UNIQUE" based on the concatenation key in column G with the formula =IF(COUNTIF($G$2:G2, G2) > 1, "DUPLICATE", "UNIQUE").

- (Column I) total_duplicates: Counts how many duplicate rows exist in column H, resulting in a total of 61685, with the formula =COUNTIF(H2:H89317, "DUPLICATE").

- (Column J) total_missing_values: Counts the number of missing values in the dataset, resulting in a total of 368, with the formula =COUNTBLANK(A2:F89317).

- (Column K) total_missing_category_name: Counts the number of rows with missing product category name, resulting in 308, with the formula =COUNTBLANK(B2:B89317).

Standardized and corrected product category names for consistency and accuracy through the table:

- “perfumery” → “perfumes”

- “telephony” → “telephones”

- “home_confort” → “home_comfort”

- “fixed_telephony” → “fixed_telephones”

- “fashio_female_clothing” → “fashion_female_clothing”

<br>

## Tools & Techniques
- **Kaggle Notebook**: Utilized Python to load files, preview data, and identify missing values and duplicates.
- **Microsoft Excel**: Perform ETL processes and validation using the results from the Python code using Power Query, formulas (e.g., COUNTBLANK, IF), and conditional formatting.
- **SQL Server Express & SSMS**: Wrote SQL queries to demonstrate core project work and KPIs.
- **Power BI**: Created dashboard visualization and DAX calculations to showcase metrics and KPIs from the data analysis.

<br>

## Project Structure
```
Order_Fulfillment_Process_Optimization_Project/
├── 1_data/
│   ├── original_data_formatted.xlsx
│   ├── formatted_and_cleaned_data.xlsx
│   └── train/
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

---
# Note to self: The sections above have been updated; the sections below still need modification.
---

<br>

## Key Questions & Insights  
- What are the average and distribution of fulfillment times across different shipping methods?  
- Which stages in the process (order processing, shipping, last-mile delivery) contribute most to delays?  
- How do product types or regions affect fulfillment efficiency?  
- Forecast trends in order volume and fulfillment times to predict peak periods and capacity needs.  
- Recommendations for process improvements based on data-driven insights.

<br>

## Power BI Dashboard  
An interactive Power BI dashboard visualizes fulfillment KPIs, bottlenecks, and forecasted trends. It leverages DAX for dynamic calculations such as average delay per stage, percentage of on-time deliveries, and rolling forecast metrics.

Dashboard features:
- Drill-down by shipping method, product category, and region.
- Time series analysis of fulfillment times and delays.
- Forecast visuals for upcoming order volume and resource planning.

<br>

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
