# DataOps Pipeline Setup 🛠️

This guide provides **step-by-step instructions** to set up **MinIO, Airflow, PostgreSQL**, and configure **git-sync** for CI/CD.

---

## 1️⃣ **MinIO Setup**
1. **Run MinIO container**:
   ```bash
   docker run -d --name minio -p 9000:9000 -p 9001:9001 -e "MINIO_ROOT_USER=admin" -e "MINIO_ROOT_PASSWORD=password" quay.io/minio/minio server /data --console-address ":9001"
   ```
2. **Access MinIO Console** → `http://localhost:9001/` (Login: `admin` / `password`).
3. **Create `dataops-bronze` bucket** and upload `dirty_store_transactions.csv`.
4. **Verify upload**:
   ```bash
   mc alias set myminio http://localhost:9000 admin password
   mc ls myminio/dataops-bronze/raw/
   ```

---

## 2️⃣ **PostgreSQL Setup**
1. **Run PostgreSQL container**:
   ```bash
   docker run --name postgres -e POSTGRES_USER=airflow -e POSTGRES_PASSWORD=airflow -e POSTGRES_DB=traindb -p 5432:5432 -d postgres:15
   ```
2. **Create required table**:
   ```sql
   CREATE TABLE IF NOT EXISTS clean_data_transactions (
       store_id TEXT,
       store_location TEXT,
       product_category TEXT,
       product_id INTEGER,
       mrp FLOAT,
       cp FLOAT,
       discount FLOAT,
       sp FLOAT,
       date_casted DATE
   );
   ```

---

## 3️⃣ **Airflow Setup**
1. **Run Airflow containers**:
   ```bash
   docker-compose up -d
   ```
2. **Access Airflow UI** → `http://localhost:8080/` (Login: `airflow` / `airflow`).
3. **Configure PostgreSQL connection** (`Admin → Connections → Add Connection`):
   - Connection ID: `postgresql_conn`
   - Host: `postgres`
   - Port: `5432`
   - Schema: `traindb`
   - Username: `airflow`
   - Password: `airflow`.

4. **Enable DAG** in Airflow UI (`dataops_pipeline`).

---

## 4️⃣ **Git-Sync for CI/CD**
1. **Ensure `git-sync` is running in Airflow & Spark containers**:
   ```yaml
   environment:
     - GIT_SYNC_REPO=<REPO_URL>
     - GIT_SYNC_BRANCH=main
   volumes:
     - git-repo:/repo
   ```
2. **Pipeline triggers automatically on `main` branch update.**

---

✅ **Now, your DataOps pipeline is ready!** 