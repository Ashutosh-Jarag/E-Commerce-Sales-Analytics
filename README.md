# 🛒 E-Commerce Sales Analytics Data Warehouse

[![PySpark](https://img.shields.io/badge/PySpark-3.5-orange)](https://spark.apache.org/)
[![Delta Lake](https://img.shields.io/badge/Delta%20Lake-3.0-blue)](https://delta.io/)
[![Databricks](https://img.shields.io/badge/Databricks-Serverless-red)](https://databricks.com/)
[![SQL](https://img.shields.io/badge/SQL-Advanced-green)](https://www.sql.org/)

A production-ready data warehouse built using **Star Schema** design pattern to analyze Brazilian e-commerce sales data. This project demonstrates end-to-end data engineering skills including ETL pipeline development, dimensional modeling, and advanced analytics.

---

## 📊 Project Overview

Built a scalable data warehouse processing **113,000+ transactions** from a Brazilian e-commerce marketplace to enable fast, reliable business intelligence and analytics.

### Key Features
- ⭐ **Star Schema Design** with 1 fact table and 4 dimension tables
- 💾 **Delta Lake Storage** for ACID transactions and time travel capabilities
- ⚡ **Optimized Performance** with Z-ordering and data skipping
- 📈 **Business Analytics** with 10+ complex SQL queries
- 🔄 **ETL Pipeline** processing 8 source tables into unified analytics layer

---

## 🏗️ Architecture

### Star Schema Design

```
                    DIM_CUSTOMER
                    (99,441 rows)
                          |
                          | customer_key
                          |
    DIM_PRODUCT -----> FACT_SALES <----- DIM_DATE
    (32,951 rows)   (113,314 rows)    (1,461 rows)
                          |
                          | seller_key
                          |
                     DIM_SELLER
                     (3,095 rows)
```

### Data Flow

```
Source Data (CSV)
    │
    ├── Orders (99K rows)
    ├── Order Items (113K rows)
    ├── Customers (99K rows)
    ├── Products (33K rows)
    ├── Sellers (3K rows)
    ├── Payments (104K rows)
    ├── Reviews (104K rows)
    └── Geolocation (1M rows)
    │
    ▼
PySpark ETL Pipeline
    │
    ├── Data Cleaning
    ├── Transformation
    ├── Surrogate Key Generation
    ├── Star Schema Modeling
    └── Data Quality Checks
    │
    ▼
Delta Lake Storage
    │
    ├── FACT_SALES
    ├── DIM_CUSTOMER
    ├── DIM_PRODUCT
    ├── DIM_SELLER
    └── DIM_DATE
    │
    ▼
Analytics & Insights
```

---

## 🛠️ Technology Stack

| Layer | Technology |
|-------|-----------|
| **Compute Engine** | Apache Spark (PySpark) |
| **Storage Format** | Delta Lake (Parquet + Transaction Log) |
| **Platform** | Databricks (Serverless) |
| **Query Language** | SQL, PySpark DataFrame API |
| **Data Modeling** | Star Schema, Dimensional Modeling |
| **Version Control** | Git, GitHub |

---

## 📁 Project Structure

```
ecommerce-data-warehouse/
│
├── notebooks/
│   ├── 01_data_loading.ipynb           # Load source CSV files
│   ├── 02_data_exploration.ipynb       # EDA and data profiling
│   ├── 03_dimension_tables.ipynb       # Create dimension tables
│   ├── 04_fact_table.ipynb             # Create fact table
│   ├── 05_delta_lake_save.ipynb        # Persist to Delta Lake
│   └── 06_business_analytics.ipynb     # SQL analytics queries
│
├── data/
│   ├── raw/                            # Original CSV files
│   └── delta/                          # Delta Lake tables
│
├── sql/
│   └── analytics_queries.sql           # Business analytics SQL
│
├── docs/
│   ├── schema_diagram.png              # Star schema visualization
│   └── data_dictionary.md              # Column descriptions
│
├── README.md
└── requirements.txt
```

---

## 📊 Data Model

### Fact Table: FACT_SALES
**Grain:** One row per item sold in each order

| Column | Type | Description |
|--------|------|-------------|
| sale_key | INT | Primary key (surrogate) |
| order_id | STRING | Original order identifier |
| order_item_id | INT | Item sequence in order |
| customer_key | INT | Foreign key → DIM_CUSTOMER |
| product_key | INT | Foreign key → DIM_PRODUCT |
| seller_key | INT | Foreign key → DIM_SELLER |
| date_key | INT | Foreign key → DIM_DATE |
| price | DECIMAL(10,2) | Product price |
| freight_value | DECIMAL(10,2) | Shipping cost |
| total_amount | DECIMAL(10,2) | Total transaction value |
| payment_value | DECIMAL(10,2) | Amount paid |
| payment_installments | INT | Number of installments |
| review_score | INT | Customer rating (1-5) |

### Dimension Tables

**DIM_CUSTOMER** (SCD Type 1)
- customer_key, customer_id, city, state, zip_code, segment

**DIM_PRODUCT** (SCD Type 1)
- product_key, product_id, category, weight, dimensions, photos_qty

**DIM_SELLER** (SCD Type 1)
- seller_key, seller_id, city, state, zip_code

**DIM_DATE** (Static)
- date_key, full_date, year, quarter, month, day_name, is_weekend

---

## 🚀 Getting Started

### Prerequisites
```bash
- Databricks account (Community Edition or higher)
- Python 3.8+
- PySpark 3.5+
- Access to Brazilian E-Commerce dataset
```

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/ecommerce-data-warehouse.git
cd ecommerce-data-warehouse
```

2. **Upload to Databricks**
- Import notebooks to Databricks workspace
- Upload CSV files to `/Volumes/` or `/FileStore/`

3. **Run notebooks in order**
```
01_data_loading → 02_data_exploration → 03_dimension_tables 
→ 04_fact_table → 05_delta_lake_save → 06_business_analytics
```

### Quick Start (Load Existing Warehouse)

```python
# Load all tables from Delta Lake
BASE_PATH = "/Volumes/sales_analysis/data/data/"

fact_sales = spark.read.format("delta").load(BASE_PATH + "fact_sales_delta")
dim_customer = spark.read.format("delta").load(BASE_PATH + "dim_customer_delta")
dim_product = spark.read.format("delta").load(BASE_PATH + "dim_product_delta")
dim_seller = spark.read.format("delta").load(BASE_PATH + "dim_seller_delta")
dim_date = spark.read.format("delta").load(BASE_PATH + "dim_date_delta")

# Register as SQL views
fact_sales.createOrReplaceTempView("fact_sales")
dim_customer.createOrReplaceTempView("dim_customer")
dim_product.createOrReplaceTempView("dim_product")
dim_seller.createOrReplaceTempView("dim_seller")
dim_date.createOrReplaceTempView("dim_date")

print("✅ Data warehouse loaded!")
```

---

## 📈 Business Analytics Examples

### 1. Monthly Revenue Trend
```sql
SELECT 
    d.year,
    d.month_name,
    ROUND(SUM(f.total_amount), 2) AS revenue,
    COUNT(DISTINCT f.order_id) AS total_orders
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
GROUP BY d.year, d.month, d.month_name
ORDER BY d.year, d.month;
```

### 2. Top Product Categories
```sql
SELECT 
    p.product_category_name,
    COUNT(*) AS units_sold,
    ROUND(SUM(f.total_amount), 2) AS revenue,
    ROUND(AVG(f.review_score), 2) AS avg_rating
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY p.product_category_name
ORDER BY revenue DESC
LIMIT 10;
```

### 3. Customer Lifetime Value
```sql
SELECT 
    c.customer_state,
    COUNT(DISTINCT c.customer_key) AS customers,
    ROUND(AVG(total_spent), 2) AS avg_lifetime_value
FROM (
    SELECT 
        customer_key,
        SUM(total_amount) AS total_spent
    FROM fact_sales
    GROUP BY customer_key
) customer_totals
JOIN dim_customer c ON customer_totals.customer_key = c.customer_key
GROUP BY c.customer_state
ORDER BY avg_lifetime_value DESC;
```

---

## 🎯 Key Insights Discovered

- 📊 **Revenue:** R$ 15.9M total sales across 113K transactions
- 💰 **AOV:** Average order value of R$ 140.46
- ⭐ **Satisfaction:** 3.99/5 average customer rating
- 📍 **Geography:** São Paulo (SP) accounts for 42% of customers
- 📦 **Top Category:** Bed/Bath/Table products lead in revenue
- 💳 **Payments:** 70% of customers use 1-3 installments
- 📅 **Seasonality:** Q4 shows 35% higher sales than Q1

---

## 🔧 Technical Highlights

### ETL Pipeline Features
- **Data Quality Checks:** NULL handling, data type validation, constraint verification
- **Surrogate Key Generation:** Sequential integer keys for all dimensions
- **Incremental Loading Ready:** Architecture supports future incremental updates
- **Error Handling:** Comprehensive try-catch blocks and logging

### Performance Optimizations
- **Z-Ordering:** Optimized `date_key` for faster date-range queries
- **Delta Lake Optimization:** Compacted small files into larger ones
- **Partition Strategy:** Logical partitioning by date for scalability
- **Caching:** Strategic use of DataFrame caching for repeated operations

### Delta Lake Advantages
- **ACID Transactions:** Guaranteed data consistency
- **Time Travel:** Query historical versions of data
- **Schema Evolution:** Seamlessly handle schema changes
- **Audit Trail:** Complete transaction log history

---

## 📚 Data Dictionary

Detailed descriptions of all tables and columns available in [`docs/data_dictionary.md`](docs/data_dictionary.md)

---

## 🎓 Skills Demonstrated

### Data Engineering
- ✅ Dimensional Modeling (Star Schema)
- ✅ ETL Pipeline Development
- ✅ Data Warehouse Design
- ✅ Slowly Changing Dimensions (SCD)
- ✅ Surrogate Key Management

### Technical Skills
- ✅ PySpark (DataFrames, SQL, Window Functions)
- ✅ Delta Lake (ACID, Time Travel, Optimization)
- ✅ SQL (Joins, Aggregations, CTEs, Window Functions)
- ✅ Databricks Platform
- ✅ Data Quality & Validation

### Business Acumen
- ✅ KPI Definition & Calculation
- ✅ Business Intelligence
- ✅ Analytical Query Design
- ✅ Data-Driven Insights

---

## 📊 Performance Metrics

| Metric | Value |
|--------|-------|
| **Total Records Processed** | 1M+ rows |
| **Data Warehouse Size** | ~500 MB (Delta format) |
| **Load Time (CSV → Delta)** | ~2 minutes |
| **Query Performance** | <5 seconds for complex aggregations |
| **Data Quality** | 100% key integrity, 0 NULL foreign keys |

---

## 🚧 Future Enhancements

- [ ] Implement incremental data loading (CDC - Change Data Capture)
- [ ] Add data quality framework (Great Expectations)
- [ ] Create automated ETL orchestration (Apache Airflow/Databricks Workflows)
- [ ] Build real-time dashboard (Plotly Dash / Streamlit)
- [ ] Add machine learning models (Customer Churn, Product Recommendations)
- [ ] Implement SCD Type 2 for historical tracking
- [ ] Add data lineage tracking
- [ ] Create dbt models for transformation layer

---

## 📝 Dataset Source

**Brazilian E-Commerce Public Dataset by Olist**
- Source: [Kaggle - Brazilian E-Commerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- Size: 100K orders from 2016-2018
- License: CC BY-NC-SA 4.0

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📧 Contact

**Your Name**
- Email: your.email@example.com
- LinkedIn: [linkedin.com/in/yourprofile](https://linkedin.com/in/yourprofile)
- GitHub: [@yourusername](https://github.com/yourusername)
- Portfolio: [yourportfolio.com](https://yourportfolio.com)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Olist for providing the public dataset
- Databricks Community Edition for compute resources
- Apache Spark & Delta Lake open-source communities

---

## ⭐ Star This Repository

If you found this project helpful, please give it a ⭐ on GitHub!

---

**Built with ❤️ using PySpark, Delta Lake, and Databricks**