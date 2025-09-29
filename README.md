# 📊 Sales Analytics on Databricks

## 🚀 Project Overview
This project implements a **Sales Analytics Platform** on **Databricks**, designed for **real-time ingestion**, **data quality management**, and **business-ready analytics**.  

Key features:
- Data ingestion from **AWS S3** (source CRM & ERP systems).
- Processing with **Databricks Auto Loader** using **Structured Streaming**.
- Data organization via the **Medallion Architecture (Bronze → Silver → Gold)**.
- Data modeling into a **Star Schema** for BI & dashboarding.
- Optimized for **scalable analytics** and **real-time insights**.

---

## 🏗️ Architecture

### 🔹 Medallion Layers
1. **Bronze (Raw Layer)**  
   - Ingests raw files (`CSV`) from S3 (`source_crm` & `source_erp`).  
   - Retains original data for traceability.

2. **Silver (Cleansed & Enriched Layer)**  
   - Cleans data (handling missing values, standardizing formats).  
   - Joins CRM & ERP sources into consolidated **Sales Fact Data**.  
   - Enforces data quality constraints.

3. **Gold (Business-Ready Layer)**  
   - Star schema with **Fact** and **Dimension** tables.  
   - Optimized for BI dashboards & self-service analytics.

---

## 📂 Data Sources
- **`s3://databrick-data-engineer/source_crm/`**  
  Contains customer and sales-related data from CRM (e.g., `cust-info.csv`).

- **`s3://databrick-data-engineer/source_erp/`**  
  Contains product, order, and financial data from ERP (e.g., `sales-orders.csv`, `products.csv`).

---

## ⚙️ Data Ingestion
Data ingestion leverages **Databricks Auto Loader** with **path glob filtering** to continuously stream only relevant files into the **Bronze layer**.

```python
df = (
    spark.readStream
        .format("cloudFiles")
        .option("cloudFiles.format", "csv")
        .option("header", "true")
        .option("cloudFiles.schemaLocation", "dbfs:/mnt/schemas/source_crm")
        .option("pathGlobFilter", "*cust-info.csv")
        .load("s3://databrick-data-engineer/source_crm/")
)
