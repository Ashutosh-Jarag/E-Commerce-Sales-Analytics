E-Commerce Sales Analytics (Olist Dataset)

This project performs end-to-end data engineering & analytics on the Brazilian E-Commerce (Olist) dataset using Databricks, PySpark, SQL, Delta Lake, and Dimensional Modeling (Star Schema).

🚀 Project Overview
Phase	What we completed
🧹 Data Loading	Loaded 9 CSV files into Databricks
🔍 Data Exploration	Displayed schemas, row counts & sample rows
⭐ Data Modeling	Designed & built a Star Schema
🗓️ Dimension Tables	dim_date, dim_customer, dim_seller, dim_product, etc.
📦 Fact Table	fact_order_items containing sales metrics
🛑 Delta Lake Storage	Stored all dimensions + fact tables in Delta format
📈 Analytics Queries	Ran real business questions using SQL


🛠️ Technology Stack
Component	Purpose
Databricks	Unified Analytics Platform
PySpark	ETL + Transformations
Delta Lake	Reliable storage format
SQL	BI analytics & reporting
Star Schema	Dimensional data model for fast queries



📂 Project Folder Structure
notebooks/
│── 01_data_loading.ipynb
│── 02_data_exploration.ipynb
│── 03_star_schema_build.ipynb
│── 04_delta_tables_write.ipynb
│── 05_analytics_queries.ipynb
data/
│── raw CSV uploaded to Volumes
delta/
│── fact & dimension delta tables
README.md


⭐ Star Schema (Final Data Model)
                    dim_customer
                         |
dim_seller — fact_order_items — dim_product — dim_category
                         |
                    dim_date
