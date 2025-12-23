# Data Engineering Project with Microsoft Fabric – Earthquake Analytics

## Project Overview

This project demonstrates an end-to-end **data engineering pipeline on Microsoft Fabric** using the **USGS Worldwide Earthquake Events API**. The solution follows a **Medallion Architecture (Bronze, Silver, Gold)** and is fully automated using **Microsoft Fabric Data Factory**, with analytics and visualization delivered through **Power BI (Direct Lake)**.

The pipeline ingests daily earthquake data, processes it incrementally, and publishes business-ready datasets for reporting without duplicating data.

---

## Architecture Overview

**Medallion Architecture (Lakehouse-based):**

- **Bronze Layer**: Raw API ingestion (JSON)
- **Silver Layer**: Cleaned and structured data (Delta tables)
- **Gold Layer**: Enriched, analytics-ready data (Delta tables)

The **Fabric Lakehouse** acts as the central data repository, combining the scalability of a data lake with the performance and governance of a data warehouse.

---

## Microsoft Fabric Components Used

- **Fabric Data Engineering** – Lakehouse, PySpark notebooks
- **Fabric Data Factory** – Pipeline orchestration and scheduling
- **Fabric Lakehouse** – Delta storage with versioning and transactions
- **SQL Analytics Endpoint** – SQL-based querying and EDA
- **Semantic Model (Direct Lake)** – Power BI integration without data duplication
- **Power BI** – Interactive dashboards and geospatial reporting

---

## Workspace & Lakehouse Setup

- Created a Fabric workspace named ``
- Created a Lakehouse named `` under the **Data Engineering experience**
- Delta format is used by default for tables, enabling:
  - ACID transactions
  - Efficient updates and merges
  - Time travel and rollback

---

## Data Source

**USGS Earthquake Catalog API**

- Supports query parameters such as `starttime` and `endtime`
- Returns data in GeoJSON format
- Allows dynamic, parameterized ingestion for incremental loads

---

## Bronze Layer – Raw Data Ingestion

### Purpose

- Ingest raw earthquake data with minimal transformation
- Preserve original structure for traceability and reprocessing

### Processing Logic

- API is called using dynamic date parameters
- Data is stored as JSON files in the Lakehouse `Files` area
- This layer acts as the immutable source of truth

### Key Features

- Parameterized ingestion via Data Factory
- Daily incremental loads
- Raw JSON retained for auditing

---

## Silver Layer – Data Cleaning & Transformation

### Purpose

- Convert raw JSON into structured, queryable format
- Prepare data for analytical use

### Transformations

- Extracted nested GeoJSON fields
- Flattened coordinates (latitude, longitude, elevation)
- Converted epoch timestamps to Spark `TimestampType`
- Selected relevant analytical attributes

### Output

- Delta table: ``
- Clean, structured, analytics-ready schema

---

## Gold Layer – Business Enrichment

### Purpose

- Create business-ready datasets optimized for reporting

### Enrichments

- Reverse geocoding to derive **country codes** from latitude/longitude
- Earthquake significance classification:
  - **Low**: sig < 100
  - **Moderate**: 100 ≤ sig < 500
  - **High**: sig ≥ 500

### Output

- Delta table: ``
- Optimized for Power BI dashboards and insights

---

## Data Pipeline Automation

### Orchestration (Fabric Data Factory)

- Notebook activities for:
  - Bronze ingestion
  - Silver transformation
  - Gold enrichment
- Dynamic parameters passed into notebooks:
  - `start_date = utcnow() - 1 day`
  - `end_date = utcnow()`

### Capabilities

- Fully automated daily refresh
- Incremental data ingestion
- End-to-end dependency management
- Schedulable for specific execution times

---

## Analytics & Power BI Integration

### Semantic Model (Direct Lake)

- Automatically created from the Lakehouse
- Uses **Direct Lake**, avoiding data import or duplication
- Always in sync with Delta tables

### Reporting

- SQL Analytics Endpoint used for validation and EDA
- Gold tables added to the default semantic model
- Power BI report built using:
  - Map visuals for earthquake locations
  - Time-based and severity-based analysis

---

## Technologies Used

- **Python**
- **PySpark**
- **Microsoft Fabric**
  - Data Engineering
  - Data Factory
  - Lakehouse
  - SQL Analytics
  - Power BI (Direct Lake)

---

## Project Highlights

- End-to-end Fabric-native data engineering solution
- Lakehouse-based Medallion Architecture
- Fully automated, parameterized pipelines
- No data duplication between Lakehouse and Power BI
- Production-ready design aligned with enterprise data platforms

---

## Future Enhancements

- Streaming ingestion using Eventstream or Kafka
- Slowly Changing Dimensions (SCD) for historical analysis
- Advanced anomaly detection on seismic activity
- CI/CD integration for Fabric artifacts

---


