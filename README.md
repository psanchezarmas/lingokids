# Lingokids Data Engineering Challenge

This repository implements a medallion architecture (Bronze → Silver → Gold) for the Lingokids analytics challenge, using Databricks notebooks, Delta Lake, and configuration-driven ETL.

---

## Project Structure

```
lingokids/
├── README.md
├── .github/
├── .vscode/
├── analytics/
│   └── analytics_engineer_challenge.ipynb
├── configs/
│   ├── bronze/
│   ├── silver/
│   └── gold/
├── notebooks/
│   ├── bronze_ingestion/
│   ├── silver_ingestion/
│   └── gold_ingestion/
├── resources/
│   ├── samples/
│   └── schemas/
├── setup/
│   └── setup.ipynb
└── tests/
```

---

## Getting Started

### 1. Environment Setup

- **Platform:** Databricks (recommended: DBR 13+)
- **Languages:** Python (PySpark), SQL
- **Dependencies:**  
  - `pycountry`, `pycountry_convert`, `pytz`, `pandas`, `numpy`
  - Delta Lake (Databricks default)

Install any missing libraries using `%pip install` in a notebook cell, e.g.:
```python
%pip install pycountry pycountry_convert pytz pandas numpy
```

---

### 2. Catalog and Schema Initialization

Run `setup/setup.ipynb` to:
- Create the Unity Catalog and medallion schemas (bronze, silver, gold) as defined in `setup.yaml`.
- This notebook uses Databricks REST API and Spark SQL to manage catalog objects.

---

### 3. Bronze Layer Ingestion

- **Config-driven ingestion:**  
  - YAML files in `configs/bronze/` define table schemas, source JSON files, and Spark types.
  - See `notebooks/bronze_ingestion/bronze_ingestion_layer.ipynb` for the main ingestion logic.
- **How to run:**  
  - Execute the notebook to ingest all JSON files into Delta tables in the `bronze` schema.
  - The ingestion is incremental/configurable via YAML.

---

### 4. Silver Layer ETL

- **Purpose:** Clean, normalize, and enrich data from Bronze.
- **Notebooks:**  
  - `notebooks/silver_ingestion/activities_silver.ipynb`
  - `notebooks/silver_ingestion/events_silver.ipynb`
  - `notebooks/silver_ingestion/users_silver.ipynb`
  - `notebooks/silver_ingestion/dates_silver.ipynb`
  - `notebooks/silver_ingestion/countries_silver.ipynb`
- **Features:**  
  - Merge/upsert logic to prevent duplicates.
  - Data enrichment (e.g., date dimension, country info).
  - Config-driven (YAML in `configs/silver/`).

---

### 5. Gold Layer Aggregation

- **Purpose:** Build analytics-ready tables for dashboards and reporting.
- **Notebook:**  
  - `notebooks/gold_ingestion/dashboard_ingestion.ipynb`
- **Features:**  
  - Joins Silver tables for denormalized reporting.
  - Aggregates metrics (activity counts, durations, completion rates, etc.).
  - Supports slicing by date, country, device, subscription status, etc.

---

### 6. Analytics & Challenge Solutions

- **Notebook:**  
  - `analytics/analytics_engineer_challenge.ipynb`
- **Contents:**  
  - SQL queries to answer the challenge questions using the Gold/Silver tables.

---

## Configuration

- **YAML files** in `configs/` define table schemas, source files, and ingestion logic.
- **JSON schemas** in `resources/schemas/` are used for validation.

---

## Running the Pipeline

1. **Run `setup/setup.ipynb`** to create catalog and schemas.
2. **Run `notebooks/bronze_ingestion/bronze_ingestion_layer.ipynb`** to ingest raw data into Bronze.
3. **Run Silver notebooks** in `notebooks/silver_ingestion/` to process and upsert data into Silver tables.
4. **Run Gold notebook** `notebooks/gold_ingestion/dashboard_ingestion.ipynb` to generate analytics tables.
5. **Explore and analyze** using `analytics/analytics_engineer_challenge.ipynb`.

---

## Testing

- Place test scripts and validation notebooks in the `tests/` directory.

---

## Notes

- All ETL is designed to be idempotent and incremental where possible.
- The pipeline is fully configuration-driven for easy extension.
- For any schema or config changes, update the corresponding YAML and rerun the relevant notebook.

---

## Contact

For questions or issues, please reach out to the repository owner.