# 🏭 FMCG Data Engineering Project — Lakehouse Consolidation Pipeline

> An end-to-end data engineering project simulating a real-world M&A scenario where a large FMCG retail company acquires a smaller one. The goal is to build a full ETL pipeline that consolidates data from both companies into a single unified Lakehouse using Medallion Architecture.

---

## 📐 Architecture

![Project Architecture](resources\project_architecture.png)

The pipeline follows the **Medallion Architecture** pattern:

- **Source Layer** — Raw CSV files stored in DBFS (simulating AWS S3)
- **Bronze Layer** — Raw ingested data, no transformations
- **Silver Layer** — Cleaned, standardized, and validated data
- **Gold Layer** — Star schema modeled data, ready for analytics
- **Serving Layer** — Databricks Dashboard + Genie AI

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **Apache Spark / PySpark** | Distributed data processing |
| **Databricks (Free Edition)** | Notebooks, Jobs, Dashboard, Genie |
| **Delta Lake** | ACID-compliant storage format |
| **DBFS** | Cloud storage simulation (replacing AWS S3) |
| **Unity Catalog** | Data governance and catalog management |
| **SQL** | Data transformation and modeling |
| **Python** | Pipeline scripting and ingestion logic |

---

## 🗂️ Project Structure

```
fmcg (Unity Catalog)
├── bronze_child/        ← Raw child company data (Delta tables)
│   ├── customers
│   ├── products
│   ├── pricing
│   └── orders
│
├── silver_child/        ← Cleaned and standardized data
│   ├── customers
│   ├── products
│   ├── pricing
│   └── orders
│
├── gold_child/          ← Star schema modeled data
│   ├── dim_customers
│   ├── dim_products
│   ├── dim_date
│   └── fact_orders
│
└── gold/                ← Merged parent + child (final unified layer)
    ├── dim_customers
    ├── dim_products
    ├── dim_date
    └── fact_orders
```

---

## 🔄 Pipeline

![Pipeline](dashboarding\pipeline.png)

The pipeline is orchestrated using **Databricks Jobs** with 4 sequential tasks:

```
dim_processing_customers
        ↓
dim_processing_products
        ↓
dim_processing_pricing
        ↓
fact_processing (Incremental Load)
```

Each task depends on the previous one to ensure referential integrity across dimension and fact tables. The pipeline uses **incremental loading** — only new or changed records are processed on each run.

---

## 📊 Dashboard

![Dashboard](dashboarding\dashboard.png)

A unified **Sales Insights Dashboard** built in Databricks showing:

- 💰 **Total Revenue** — ₹84.86B
- 📦 **Total Quantity Sold** — 30.45M units
- 👥 **Unique Customers** — 53
- 💵 **Average Selling Price** — ₹1.45M
- 📊 Top 10 Products by Revenue
- 🥧 Revenue by Channel (Retailer, Direct, Acquisition)
- 📈 Monthly Revenue Trend

---

## 🔀 Data Flow

```
DBFS Raw Files (Child Company)
        ↓
   Bronze Layer       ← Raw Delta tables
        ↓
   Silver Layer       ← Cleaned & standardized
        ↓
   Gold Layer         ← Star schema (dim + fact)
        ↓
   Merge with Parent Company Gold
        ↓
   Serving Layer      ← Dashboard + Genie AI
```

---

## 📁 Data Model — Star Schema

```
                  dim_customers
                       |
dim_date ——— fact_orders ——— dim_products
```

| Table | Type | Description |
|---|---|---|
| `fact_orders` | Fact | Central transaction table with all order records |
| `dim_customers` | Dimension | Customer master data |
| `dim_products` | Dimension | Product catalog with pricing |
| `dim_date` | Dimension | Date table for time-based analysis |

---

## 💡 Key Learnings

- Designing a **Medallion Architecture** from scratch for a real business scenario
- Building **incremental load pipelines** using Delta Lake merge operations
- **Star schema modeling** with proper dimension and fact table relationships
- Orchestrating multi-task pipelines using **Databricks Jobs**
- Simulating **cloud storage ingestion** using DBFS as an S3 replacement
- Building **BI dashboards** directly from Gold layer Delta tables

---

## 📌 Notes

> In a production environment, DBFS would be replaced with **AWS S3** or **Azure Data Lake Storage Gen2**. The pipeline code requires only a path change to switch between storage layers — the transformation and modeling logic remains identical.

---

## 👤 Author

**Yousef Ahmed**
Data Engineering Student

📧 yousefahmed.ae20@gmail.com