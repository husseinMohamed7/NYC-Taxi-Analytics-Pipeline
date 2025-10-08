# 🚕 NYC Yellow Taxi Analytics Pipeline

## End-to-End Data Engineering Solution for TLC Trip Records

This project establishes a scalable, end-to-end data pipeline designed to ingest, transform, and analyze millions of **NYC Yellow Taxi trip records** published by the Taxi and Limousine Commission (TLC).

The solution utilizes a modern **ELT (Extract, Load, Transform)** architecture, centered around **Snowflake** and orchestrated by **dbt**, to deliver actionable insights through a **Power BI** dashboard.

---

## 🌟 Architecture Overview

The pipeline follows a multi-stage data flow, ensuring data quality and analytical readiness:

1.  **Source & Extraction (E):** Raw TLC trip data (Parquet files) is ingested from SQL Server and PostgreSQL sources using **Python-based ETL scripts**.
2.  **Cloud Data Warehouse (L):** The raw data is loaded into the **Snowflake** cloud data warehouse (Landing/Staging layers).
3.  **Transformation (T):** **dbt (data build tool)** is used to define and execute modular transformations, building up a clean, reliable **Star Schema Data Mart**.
4.  **Visualization:** The final, optimized data mart is connected to **Power BI** for interactive analysis and business reporting.


| Source Data ➡️ | Python ETL ➡️ | Snowflake ➡️ | dbt ➡️ | Power BI ➡️ | End Users |
| :---: | :---: | :---: | :---: | :---: | :---: |
| SQL Server, PostgreSQL | Data Ingestion | Landing, Staging | Transformations | Dashboards | Insights |

---

## 🎯 Star Schema Data Model

The data mart is implemented as a **Star Schema**, optimized for analytical querying, performance, and ease of use in Power BI. The model is centered around a single fact table: `fact_trip`.

### ➡️ Fact Table

| Table Name | Primary Key (PK) | Foreign Keys (FK) | Key Measures/Attributes |
| :--- | :--- | :--- | :--- |
| **`fact_trip`** | `trip_id` | `vendor_id`, `datetime_id`, `pickup_location_id`, `dropoff_location_id`, `payment_type_id`, etc. | `fare_amount`, `tip_amount`, `total_amount`, `trip_distance` |

### ➡️ Dimension Tables

| Table Name | Description |
| :--- | :--- |
| **`dim_datetime`** | Time components (Hour, Day, Month, Year, Weekday) for both pickup and dropoff. |
| **`dim_vendor`** | Details about the taxi vendor. |
| **`dim_passenger_count`** | Passenger count categorization. |
| **`dim_rate_code`** | Lookup table for Ratecode IDs and corresponding names (e.g., JFK, Negotiated fare). |
| **`dim_payment_type`** | Lookup table for payment type IDs (e.g., Credit Card, Cash). |
| **`dim_trip_distance`** | Trip distance buckets for analysis. |
| **`dim_pickup_location`** | Latitude and longitude for pickup. |
| **`dim_dropoff_location`** | Latitude and longitude for dropoff. |

---

## ✅ Key Features & Implementation Highlights

* **Automated ELT:** **Python** scripts handle downloading, reading **Parquet** files, cleaning initial trash data (nulls, impossible values), and loading into the Snowflake Landing Zone.
* **Modular dbt Transformation:** Used a **Staging → Intermediate → Marts** layered approach to ensure data lineage, reusability, and testability.
* **Incremental Loading:** Implemented incremental strategies in dbt based on pickup timestamps to efficiently process **millions of records monthly** and minimize compute cost.
* **Data Quality Checks:** Comprehensive dbt tests prevent bad data (e.g., negative fares, duplicate IDs) from polluting the final data mart.
* **Optimized Storage:** Leveraged Snowflake features like **Clustering Keys** on date and location columns for fast query execution on large datasets.
* **Interactive Analytics:** **Power BI** dashboard provides key insights into revenue per zone, peak demand hours, and payment method trends.

---

## 🧠 Key Learnings & Challenges

* **Dimensional Modeling:** Crucial in shifting from a transactional schema to an analytical Star Schema, which drastically improved dashboard performance.
* **Handling Large Parquet Volumes:** Required robust Python libraries (`pyarrow`, `pandas`) and an efficient method to upload files to the Snowflake stage.
* **Location Data Granularity:** Required custom logic to map raw latitude/longitude coordinates to meaningful NYC Taxi Zones for analytical grouping.
* **Cost Optimization:** Mastering Snowflake's **Virtual Warehouse** sizing and auto-suspend features was essential to balance high performance with budget constraints.
* **Data Quality at Scale:** Identified and cleaned over **5% of the data** with critical quality issues (e.g., zero distance trips, negative amounts) before transformation.

---

## 🚀 Getting Started (Local Setup)

To set up and run this pipeline locally, you will need the following prerequisites:

### Prerequisites

1.  **Python 3.8+**
2.  **Snowflake Account**
3.  **dbt CLI** (`dbt-core` and `dbt-snowflake` adapter)
4.  **SQL Server / PostgreSQL instance** (for initial source loading)

### Installation Steps

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/husseinMohamed7/NYC-Yellow-Taxi-Analytics-Pipeline.git](https://github.com/husseinMohamed7/NYC-Yellow-Taxi-Analytics-Pipeline.git)
    cd NYC-Yellow-Taxi-Analytics-Pipeline
    ```

2.  **Install Python Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Configure dbt:**
    * Set up your `profiles.yml` file with your Snowflake connection details. (A template will be provided in `dbt_transformations/profiles.yml.example`).

4.  **Run the ETL Process (E & L):**
    * Execute the Python ingestion script to extract data and load it into your specified database sources (SQL Server/PostgreSQL) and Snowflake's landing stage.
    
    ```bash
    python etl_scripts/main_etl.py
    ```

5.  **Run dbt Transformations (T):**
    * Navigate to the dbt project directory:
        ```bash
        cd dbt_transformations
        ```
    * Run all tests to validate source data quality:
        ```bash
        dbt test
        ```
    * Execute the entire pipeline to build the Star Schema Data Marts:
        ```bash
        dbt run
        ```

---

## 🔗 Project Resources

| Link | Description |
| :--- | :--- |
| **View on GitHub** | `https://github.com/husseinMohamed7/NYC-Yellow-Taxi-Analytics-Pipeline` |
| **TLC Dataset Source** | `https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page` |
| **Data Dictionary** | `https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf` |

---

## 👤 Contact

**Project Author: Hussein Mohamed**

| Platform | Link |
| :--- | :--- |
| **LinkedIn** | `https://www.linkedin.com/in/hussein-mohamed7/` |
| **GitHub** | `https://github.com/husseinMohamed7` |
| **Email** | `mailto:hussein7mohamed8@gmail.com` |
| **Telegram** | `https://t.me/Hussein_770` |
