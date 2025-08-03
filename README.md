# Order Fulfillment Process Optimization Project
## Note: I am currently working on this self-initiated project. I have completed the cleaning and EDA phases and am about to start building the Power BI dashboard. I will post the completed version on LinkedIn soon—stay tuned! (Updated July 2nd)

## Overview
This project explores trends in the e-commerce order fulfillment process using a multi-table dataset. It focuses on analyzing order performance, payment methods, and delivery timelines to identify process inefficiencies and customer experience gaps. Python was used for data loading, exploration, and cleaning, Excel for organization, SQL for KPI analysis, and Power BI for interactive visualization.

## Objective
Understand what drives order performance and customer experience by analyzing trends across the fulfillment lifecycle, and explore forecasting techniques to support better operational planning and business decisions.

## Data Source
The dataset used in this project is publicly available on Kaggle:
[Ecommerce Order & Supply Chain Dataset by Aditya Bagus Pratama](https://www.kaggle.com/datasets/bytadit/ecommerce-order-dataset/data)

It covers the fulfillment pipeline from order placement through delivery, including order-level data, shipping and delivery timestamps, customer and product details, shipping status, and more.

License: Provided under Kaggle Terms of Use; all rights belong to the original author.

## Content Description
The source dataset includes two folders: train and test. This project uses only the train folder, which contains five CSV files:

- df_Customers: Contains customer identifiers and regional information.

- df_OrderItems: Details the individual products within each order, including seller ID, price, and shipping charges.

- df_Orders: Order data such as order ID, order status, and shipping and delivery dates.

- df_Payments: Payment information per order, including payment type, installments, and amount.

- df_Products: Product data including category name and physical measurements.


## Data Cleaning & Preparation
- Standardized all text-based columns to uppercase and to middle alignment across all datasets for consistency and improved readability, except for `customer_city`, it is only capitalized on the first letter of each word.

- The numeric monetary columns are displayed with two decimal places for clarity, and along with the timestamp columns, they are all right aligned for easier comparison and scanning. Only the product measurement columns were kept as whole integers.

- Converted helper columns H and J in cleaned_df_orders and columns G and H in cleaned_df_products as static values to improve Excel performance and reduce loading time (~89,000 rows).


### Exploratory data analysis on cleaned_df_orders

- Applied conditional formatting to highlight blank cells with orange fill and solid outline borders for improved visibility and easier identification of missing data:

  - (Column E) `order_approved_at`: Applied to the range $E$2:$E$89317 with the formula =$J2="MISSING" to highlight flagged rows in helper column J.

  - (Column F) `order_delivered_timestamp`: Applied two formulas to the range $F$2:$F$89317:
    - =ISBLANK($F2) to highlight all blank timestamps.
    - =AND($C2="DELIVERED", ISBLANK($F2)) to flag missing timestamps for orders marked as "DELIVERED".

Added 5 helper columns to identify data quality issues and support KPI calculations:

- (Column H) `order_status_timestamp_issue`: Flags rows where `order_status` is "DELIVERED" but `order_delivered_timestamp` is missing.

  Formula: =IF(AND($C2="DELIVERED", ISBLANK($F2)), "MISSING DELIVERED TIMESTAMP", "OK")

- (Column I) `count_timestamp_issue`: Counts total flagged rows in column H.

  Result: 6

  Formula: =COUNTIF(H2:H89317, "MISSING DELIVERED TIMESTAMP")

- (Column J) `order_approved_flag`: Flags rows where `order_approved_at` is missing.

  Formula: =IF(ISBLANK(E2), "MISSING", "PRESENT")

- (Column K) `missing_order_approved`: Counts total missing approval timestamps flagged in Column J.

  Result: 9

  Formula: =COUNTIF(J2:J89317, "MISSING")

- (Column L) `missing_delivered_timestamp`: Counts total missing delivery timestamps in column F.

  Formula: =COUNTIF(F2:F89317, "")


### Exploratory data analysis on cleaned_df_products

- Identified 14 entries (13 duplicates) of the same product_id (ZX9HL81JFVR2) in the Toys category. These rows had missing product measurement data, but were retained to preserve integrity across datasets.

- Applied conditional formatting to the range $B$2:$F$89317 with the formula =ISBLANK(B2) to highlight blank cells. Blank cells are formatted with orange fill and solid outline borders for improved visibility and easier identification of missing data.

Added 5 helper columns to identify data quality issues and support KPI calculations:

- (Column G) `concatenation_key`: Combined columns A to F into a single key used in column H to detect duplicate or unique rows.

  Formula: =A2 & "|" & B2 & "|" & C2 & "|" & D2 & "|" & E2 & "|" & F2

- (Column H) `duplicate_label`: Flags rows as "DUPLICATE" or "UNIQUE" based on the key in column G.

  Formula: =IF(COUNTIF($G$2:G2, G2) > 1, "DUPLICATE", "UNIQUE")

- (Column I) `total_duplicates`: Counts how many rows are marked as "DUPLICATE" in column H.

  Result: 61,685

  Formula: =COUNTIF(H2:H89317, "DUPLICATE")

- (Column J) `total_missing_values`: Counts the total number of blank cells across columns A to F.

  Result: 368

  Formula: =COUNTBLANK(A2:F89317)

- (Column K) `missing_category_name`: Counts how many rows have a missing  `product_category_name` in column B.

  Result: 308

  Formula: =COUNTBLANK(B2:B89317)

- (Column L) `missing_product_weight_g`: Counts how many rows are blank in column C.

Result: 15

Formula: =COUNTBLANK(C2:C89317)

- (Column M) `missing_product_length_cm`: Counts how many rows are blank in column D.

Result: 15

Formula: =COUNTBLANK(D2:D89317)
  
- (Column N) `missing_product_height_cm`: Counts how many rows are blank in column E.

Result: 15

Formula: =COUNTBLANK(E2:E89317)

- (Column O) `missing_product_width_cm`: Counts how many rows are blank in column F.

Result: 15

Formula: =COUNTBLANK(F2:F89317)

Standardized and corrected `product_category_name` for consistency and accuracy throughout the dataset:

- “perfumery” → “perfumes”

- “telephony” → “telephones”

- “home_confort” → “home_comfort”

- “fixed_telephony” → “fixed_telephones”

- “fashio_female_clothing” → “fashion_female_clothing”


## Tools & Techniques
- **Kaggle Notebook:** Used Python to load files, preview data, and identify missing values and duplicates.
- **Microsoft Excel:** Perform ETL processes and data validation using Power Query, formulas (e.g., COUNTBLANK, IF), and conditional formatting based on results from Python analysis in Kaggle.
- **SQL Server Express & SSMS:** Wrote SQL queries to demonstrate core project logic and KPI generation.
- **Power BI:** Built a dashboard and used DAX calculations to perform KPI calculations and demonstrate core project logic.

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

## Key Questions & Insights  
- What are the average and distribution of fulfillment times across different shipping methods?  
- Which stages in the process (order processing, shipping, last-mile delivery) contribute most to delays?  
- How do product types or regions affect fulfillment efficiency?  
- Forecast trends in order volume and fulfillment times to predict peak periods and capacity needs.  
- Recommendations for process improvements based on data-driven insights.

## Basket Analysis Attempt
Explored the potential for basket analysis by identifying pairs of products purchased together within the same order. This was done using SQL joins on the order_items table to count co-occurring product_id pairs.

Result: No significant product pairs were found. The majority of orders contained only a single item, likely due to the short timeframe covered by the dataset.

Conclusion: Basket analysis was not applicable to this dataset due to minimal multi-item purchases.

  - Note: All SQL queries used for analysis are saved in SSMS and exported as .sql files in the repository for transparency and review.

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
