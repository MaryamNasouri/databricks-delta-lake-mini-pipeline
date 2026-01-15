# Databricks Delta Lake Mini Pipeline (Bronze / Silver / Gold)

## Project Overview
This project implements a lightweight **Delta Lake Medallion Architecture** using Databricks and Unity Catalog.

The pipeline demonstrates a modern data engineering workflow:
- Raw data ingestion (Bronze)
- Data cleaning and standardization (Silver)
- Business-ready analytics tables (Gold)

The goal is to showcase **end-to-end data pipeline design** with Delta Lake, including data quality, deduplication, and KPI generation.

---

## Architecture


Raw CSV (Unity Catalog Volume)
│
▼
Bronze Layer (Delta)
│
▼
Silver Layer (Delta)
│
▼
Gold Layer (Delta - Business KPIs)


---

## Dataset
The dataset contains raw marketing event data with the following fields:

- event_time
- customer_id
- channel
- spend
- conversion
- revenue

The raw file intentionally includes:
- duplicated records
- inconsistent channel naming
- malformed CSV formatting

This simulates real-world ingestion challenges.

---

## Bronze Layer — Raw Ingestion

**Goal:** Ingest raw data with full traceability.

### Features
- Ingest from Unity Catalog Volume
- Handle malformed CSV input
- Add ingestion metadata:
  - `ingest_time`
  - `source_file`
- Persist data as Delta table

### Output
`bronze_marketing_events` (Delta)

---

## Silver Layer — Cleaning & Standardization

**Goal:** Create a trusted, analytics-ready dataset.

### Transformations
- Type casting (timestamp, numeric)
- Channel normalization (lowercase)
- Data quality rules:
  - non-null keys
  - valid spend and revenue
  - valid conversion values
- Deduplication using business keys

### Result
- Bronze: 10 records  
- Silver: 8 trusted records  

### Output
`silver_marketing_events` (Delta)

---

## Gold Layer — Business KPIs

**Goal:** Build business-ready analytics tables.

### Aggregations
Weekly KPIs per channel:
- total_spend
- total_conversions
- total_revenue
- number of events
- ROI = revenue / spend

### Example Metrics
- Email ROI: 14.0  
- Social ROI: 5.0  
- Search ROI: 4.7  
- TV ROI: 1.17  

### Output
`gold_channel_weekly_kpis` (Delta)

---

## Technology Stack
- Databricks Community Edition
- Unity Catalog Volumes
- Apache Spark (PySpark)
- Delta Lake
- Serverless Compute

---

## Project Structure


databricks-delta-lake-mini-pipeline/

├── 01_bronze_ingest.ipynb

├── 02_silver_clean_transform.ipynb

├── 03_gold_aggregates.ipynb

├── data/

│├──  raw_marketing_events.csv

├──  README.md


---

## Key Takeaways
- Implemented a full Delta Lake Medallion pipeline
- Handled malformed CSV ingestion
- Applied data quality rules and deduplication
- Built business-ready KPI tables
- Used Unity Catalog for governed storage

---

## Business Value
This pipeline demonstrates how raw operational data can be transformed into trusted analytics datasets and executive-ready KPIs using modern lakehouse architecture.

