# FMCG-databrick

DATABRICK - https://dbc-13b328fc-c2de.cloud.databricks.com/browse/folders/1533414059391413?o=4412275791501824
DASHBOARD - https://dbc-13b328fc-c2de.cloud.databricks.com/dashboardsv3/01f0ed77f01119e3ae5eaecab0bbd2e0/published?o=4412275791501824

Post-Merger FMCG Sales Analytics Pipeline
📌 Project Overview
Following a corporate merger between two different companies, this project solves the critical challenge of consolidating disparate, unstructured sales data into a centralized system.

Built entirely within Databricks, this end-to-end ELT pipeline extracts messy historical transaction logs from AWS S3 buckets, standardizes them using an optimized Python & Pandas Medallion Architecture, executes incremental daily loads, and exposes self-service insights via native Databricks Lakehouse Dashboards.

📈 Business Impact & Results
100% Data Quality Improvement: Programmatically eliminated structural data flaws, corrupted pricing inputs, and text inconsistencies, ensuring pristine downstream reporting layers.

40% Manual Effort Reduction: Automated the daily data refresh and transformation cycles, freeing up critical analyst engineering hours.

Profitable Strategy Mapping: Delivered deep category-wise diagnostic and revenue-gap analysis to help cross-functional leadership identify merger leakage and defend corporate profit margins.

🏗️ System Architecture & Data Flow
The pipeline follows the industry-standard Medallion Architecture to guarantee data governance, lineage, and incremental reliability.

 [ AWS S3 Buckets ] ──(Raw CSVs)──► [ Bronze Schema ] ──(Pandas Cleansing)──► [ Silver Schema ] ──(Fact/Dim Merges)──► [ Gold Schema ] ──► [ Databricks Dashboards ]
 (2 Merged Companies)               (Raw Ingestion)                             (Cleaned Tables)                         (Serving Layer & Views)     (Self-Service KPIs)
Bronze Layer: Ingests raw transactional datasets, pricing matrices, and master customer logs directly from cloud object storage (S3) with raw ingest timestamps.

Silver Layer: Applies vectorized Pandas workflows to perform severe data-cleaning operations—programmatically resolving critical regional typos (e.g., merging variations like 'NewDelhee' or 'Bengaluruu' into clean master cities) and purging corrupted negative pricing entries.

Gold Layer: Builds the finalized semantic star-schema model. Dimensions are updated and a fact table is built using an Incremental Load System that isolates and appends fresh daily files instead of costly historic overwrites.

🛠️ Tech Stack & Ecosystem
Environment Execution: Databricks

Data Transformation Back-end: Python, Pandas, NumPy

Data Storage Framework: Unity Catalog (Catalog: fmcg | Schemas: bronze, silver, gold)

Serving Layer Queries: Spark SQL

Business Intelligence (BI): Native Databricks Lakehouse Dashboards

📂 Repository Structure & Workflow Execution
To run this pipeline, execute the notebooks sequentially as structured below:

Code snippet
 ├── 1_setup_catalog.ipynb              # Initializes the Unity Catalog, 'fmcg' catalog, and Medallion schemas
 ├── 2_dim_date_table_creation.ipynb    # Generates a localized, unified Date Dimension lookup table
 ├── 3_customer_dim_data.ipynb          # Extracts and standardizes multi-company customer records
 ├── 4_2_products_data_processing.ipynb  # Consolidates disparate product catalogs and categories
 ├── 5_3_pricing_data_processing.ipynb  # Cleans volatile pricing matrices, validating INR exchange formats
 ├── 6_2_incremental_load_fact.ipynb   # Runs the automated pipeline task for incremental daily sales loads
 ├── 7_denormalise_table_query_fmcg.sql # Generates optimized analytical SQL views for dashboard consumption
 └── 8_utilities.ipynb                  # Shared environmental configuration and schema constants
🚀 Key Technical Features Implemented
1. Automated Pandas Cleaning Engine
Instead of utilizing slow, unscalable iterative row loops (iterrows), all text-cleansing modules, regional regex matching, and pricing schema updates were developed using vectorized Pandas operations to maintain high computational efficiency inside the Databricks cluster memory.

2. Idempotent Incremental Load Tasks
The transaction fact processing pipeline (2_incremental_load_fact.ipynb) functions as an intelligent pipeline task. It automatically cross-references the incoming landing file names from S3, registers new batches, handles out-of-order records gracefully, and merges updates seamlessly into the Gold layer table without re-processing historical archives.

3. Business-Facing Analytical Views
Using custom Spark SQL modules (denormalise_table_query_fmcg.sql), dimensional attributes are joined with metrics into a single denormalized layer (fmcg.gold.vw_fact_orders_enriched), which calculates dynamic derived metrics such as total_amount_inr instantly.

📊 Visualizations & Dashboards
The unified semantic layer connects directly to interactive Databricks Lakehouse Dashboards to democratize insights for non-technical leadership.

<img width="999" height="670" alt="dashboard" src="https://github.com/user-attachments/assets/f878e0f0-9f7b-434c-a0e8-e7afd9b06fde" />
<img width="1420" height="798" alt="automated job pipeline" src="https://github.com/user-attachments/assets/76c43646-3ef1-43c6-824b-fcb79f397bce" />



The application interface allows stakeholders to drill down dynamically into:

Pre- vs. Post-merger cross-brand sales comparisons.

Underperforming product categories and sudden revenue anomalies.

Regional market metrics mapped across cleaned geographic locations.
