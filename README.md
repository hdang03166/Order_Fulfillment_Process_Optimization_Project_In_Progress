# Order Fulfillment Process Optimization Project

## Overview
This project shows how orders move through an e-commerce system, from when a customer places an order to when it’s delivered. This end-to-end analysis is used to discover trends, track important performance numbers (KPIs), and suggest improvements. The data is cleaned and explored using Python and Excel, applying ETL processes and exploratory data analysis. Advanced SQL queries are used to extract insights, which are then shown in a Power BI dashboard with visuals, DAX calculations, and basic forecasting to assit with future planning.

## Objective
Find problems in the order and delivery process, forecast key metrics like order volume, and provide actionable insights to optimize operations and improve the overall customer experience.

## Data Source
The dataset used in this project is publicly available on Kaggle:
[Ecommerce Order & Supply Chain Dataset by Aditya Bagus Pratama](https://www.kaggle.com/datasets/bytadit/ecommerce-order-dataset/data)

This dataset provides a view of the fulfillment pipeline from order placement through delivery. It includes order-level information such as order dates, shipping and delivery timestamps, customer and product details, shipping status, and other relevant fields. The dataset contains a train and test folder, however, only the train folder was used in this project to simplify analysis and focus on the main data.

License: Provided under Kaggle Terms of Use; all rights belong to the original author.

# Note: The sections above are finalized. The remaining parts of the project are currently in progress.

## Content Description
The train folder contains five CSV files that represent key entities involved in the e-commerce order fulfillment pipeline:

- df_Customers.csv: Contains customer identifiers and regional information.

- df_OrderItems.csv: Details the individual products within each order, including product ID, quantity, and price.

- df_Orders.csv: Captures order-level data including order date, shipping date, delivery date, and order status.

- df_Payments.csv: Logs payment information per order, including payment type and amount.

- df_Products.csv: Lists product metadata such as category and name.

For this analysis, both /train/ and /test/ datasets were merged using Python into a unified dataset to enable complete, end-to-end insights across the full order lifecycle. This consolidated dataset served as the foundation for all ETL processes, analysis, and dashboard visualizations.


## Data Cleaning & Preparation
- Utilized Python (pandas, numpy) for thorough data cleaning and preprocessing, including handling missing values, formatting dates, and engineering new features such as fulfillment time and delay indicators.

- Employed an ETL process to extract raw data, transform and enrich it, and load clean datasets into Power BI for modeling and dashboard creation.

## Tools & Techniques
- **Python**: pandas, numpy, matplotlib for data processing and forecasting.

- **Power BI**: Dashboard creation and DAX for calculated metrics and KPIs.

- **Jupyter Notebook** (via Kaggle): Documenting data cleaning and exploratory analysis.

## Project Structure
```
Order_Fulfillment_Process_Optimization_Project/
├── 1_data/
│   ├── test/
│   │   ├── df_Customers.csv
│   │   ├── df_OrderItems.csv
│   │   ├── df_Orders.csv
│   │   ├── df_Payments.csv
│   │   └── df_Products.csv
│   ├── train/
│   │   ├── df_Customers.csv
│   │   ├── df_OrderItems.csv
│   │   ├── df_Orders.csv
│   │   ├── df_Payments.csv
│   │   └── df_Products.csv
│   ├── merged_cleaned_orders.xlsx
│   └── merged_cleaned_orders.csv
├── 2_analysis/
│   └── ecommerce_data_cleaning_and_forecasting.ipynb
├── 3_powerbi/
│   └── Ecommerce_order_supply_chain_dashboard.pbix
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
