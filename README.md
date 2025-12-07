Brazilian E-Commerce Sales Analytics (Olist Dataset)

🚀 Project Overview

This project implements an end-to-end data engineering and analytics solution on the Brazilian E-Commerce Public Dataset by Olist. The goal is to transform raw, denormalized CSV data into a highly optimized, analysis-ready Star Schema using modern lakehouse architecture principles on the Databricks platform.

The final data model is stored using Delta Lake and is designed to support fast, complex analytical queries for BI, reporting, machine learning, and business forecasting.

🛠️ Technology Stack

Component

Purpose

Databricks

Unified Analytics Platform for processing and storage.

PySpark

ETL (Extract, Transform, Load) and data transformation logic.

Delta Lake

Reliable, ACID-compliant, and scalable storage format (Lakehouse).

SQL

Business Intelligence (BI) analytics, reporting, and final querying.

Star Schema

Dimensional data model for efficient analytical querying.

🏗️ Project Architecture & Workflow

The project follows a structured, modular workflow, moving data from raw ingestion to a final analytical model.

Phase

Description

Key Tools

1. Data Loading

Ingesting 9 raw Olist CSV files from cloud storage into the Databricks environment.

Databricks, PySpark

2. Data Exploration

Initial schema inspection, row counting, and sample row display to understand the dataset structure and quality.

PySpark, SQL

3. Data Modeling

Designing and building the Star Schema (Dimensional Model). This is the core transformation step.

PySpark, Dimensional Modeling

4. Delta Lake Storage

Persisting all newly created Dimension and Fact tables into the Delta Lake format for reliability and performance.

Delta Lake, PySpark

5. Analytics & Reporting

Executing real-world business queries against the optimized Delta tables using SQL.

SQL

⭐ Star Schema (Final Data Model)

The final data model is a classic Star Schema designed for optimal query performance, centered around the primary fact table, fact_order_items.

erDiagram
    fact_order_items ||--o{ dim_customer : "has"
    fact_order_items ||--o{ dim_seller : "sold_by"
    fact_order_items ||--o{ dim_product : "contains"
    fact_order_items ||--o{ dim_date : "on"
    dim_product ||--o{ dim_category : "belongs_to"


Fact Table: fact_order_items (contains sales metrics, prices, and foreign keys)

Dimension Tables: dim_date, dim_customer, dim_seller, dim_product, dim_category

📂 Project Folder Structure

.
├── notebooks/
│   ├── 01_data_loading.ipynb           # Loads all raw CSVs into Databricks DataFrames
│   ├── 02_data_exploration.ipynb       # Schema display, row counts, and data profiling
│   ├── 03_star_schema_build.ipynb      # PySpark code for creating Dimension & Fact tables
│   ├── 04_delta_tables_write.ipynb     # Writes the final tables to Delta Lake format
│   └── 05_analytics_queries.ipynb      # Contains the final business SQL queries
├── data/
│   └── raw CSV uploaded to Volumes     # Location of original Olist CSV files
└── delta/
    └── fact & dimension delta tables   # Final, persistent Delta tables


🎯 Key Outcomes

Result

Value

Final Data Model

Modern Star Schema for analytics (optimized for reporting)

Storage Format

Delta Lake (ACID properties, versioning, scalable)

Analysis Ready

Yes — Fully prepared for BI tools (Power BI, Tableau)

Supports

Dashboards, Machine Learning, forecasting & robust reporting

💡 Key Learnings

Mastering the modern data engineering workflow on Databricks.

Implementing PySpark for complex ETL and data transformation logic.

Building reliable and performant data lake tables using Delta Lake from raw CSV sources.

Designing and implementing a robust Dimensional Data Model (Star Schema).

Writing efficient SQL queries for analytical reporting on large datasets.

👨‍💻 Author

Ashutosh Jarag