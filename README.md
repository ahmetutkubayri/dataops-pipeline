# DataOps Pipeline for Store Transactions 🏪

This project implements a **DataOps pipeline** using **MinIO, Airflow, Spark, and PostgreSQL** to process and clean **daily incoming datasets**. The cleaned data is delivered to business users through PostgreSQL.

## 📌 Business Requirements
- **Ingest daily store transactions** from object storage (`MinIO`).
- **Clean data using Pandas/PySpark** (remove duplicates, handle missing values).
- **Load cleaned data into PostgreSQL (`traindb.public.clean_data_transactions`)**.
- **Automate workflow using Airflow DAG**.
- **Implement CI/CD using `git-sync` to trigger pipeline on GitHub changes**.

## 📁 Repository Structure
- `README.md` → **Project Overview**
- `SETUP.md` → **Installation & Configuration**
- `SOLUTION.md` → **Implementation Details**

---

## 🚀 Key Features
- **Data is automatically fetched from MinIO (`dataops-bronze/raw/dirty_store_transactions.csv`)**.
- **Data cleaning handles duplicates, missing values, and standardization**.
- **Transformed data is loaded into PostgreSQL (`clean_data_transactions` table)**.
- **Airflow triggers workflow automatically upon new data or Git updates**.
- **CI/CD is enabled via `git-sync` ensuring no manual deployment.**

---

## 🔗 Related Files
- [SETUP.md](SETUP.md) → Instructions on setting up MinIO, Airflow, and PostgreSQL.
- [SOLUTION.md](SOLUTION.md) → Step-by-step explanation of the implemented solution.

