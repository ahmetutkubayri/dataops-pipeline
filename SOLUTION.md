# DataOps Pipeline Solution 🚀

This document explains how the **DataOps pipeline** was implemented.

---

## **1️⃣ Data Ingestion**
- Raw data is uploaded to **MinIO (`dataops-bronze/raw/dirty_store_transactions.csv`)**.
- Airflow reads the dataset **daily** from MinIO.

---

## **2️⃣ Data Cleaning Process**
- **Handled missing values** using **forward fill**.
- **Removed duplicate rows**.
- **Ensured data types were consistent** (e.g., converting price columns to float).
- **Transformation applied in PySpark** inside the `spark_client` container.

```python
import pandas as pd

df = pd.read_csv("s3://dataops-bronze/raw/dirty_store_transactions.csv")

# Data Cleaning
df.drop_duplicates(inplace=True)
df.fillna(method='ffill', inplace=True)

# Save cleaned dataset
df.to_csv("s3://dataops-clean/clean_store_transactions.csv", index=False)
```

---

## **3️⃣ Data Load to PostgreSQL**
- **Airflow DAG** loads cleaned data into **PostgreSQL (`traindb.public.clean_data_transactions`)**.

```python
from sqlalchemy import create_engine

engine = create_engine("postgresql://username:password@host:port/traindb")
df.to_sql('clean_data_transactions', engine, schema='public', if_exists='replace', index=False)
```

---

## **4️⃣ CI/CD Implementation**
- **Airflow DAG and scripts are automatically deployed** via `git-sync`.
- **Pipeline runs daily or on new data ingestion events**.

---

✅ **Final Result: A fully automated DataOps pipeline!** 