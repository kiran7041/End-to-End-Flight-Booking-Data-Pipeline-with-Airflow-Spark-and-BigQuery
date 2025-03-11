
---


# ✈️ Flight Booking Data Pipeline with CI/CD, Airflow, Dataproc, and BigQuery  

This project demonstrates a **real-world data pipeline** that automates the **processing, transformation, and analysis** of flight booking data using **Google Cloud Platform (GCP)** services such as:  
- **Google Cloud Composer (Airflow)** for orchestration.  
- **Google Cloud Dataproc (Serverless)** for running Spark jobs.  
- **Google Cloud Storage (GCS)** for storing raw and processed data.  
- **Google Cloud BigQuery (BQ)** for storing transformed data and insights.  
- **GitHub Actions (CI/CD)** for continuous integration and deployment.

---

## **Project Overview**  

The goal of this project is to:  
1. ✅ Automate the deployment of Spark jobs and Airflow DAGs using **CI/CD pipelines**.  
2. ✅ Process flight booking data (CSV files) in **Dataproc Serverless** and store the results in **BigQuery**.  
3. ✅ Generate valuable business insights like **route insights** and **origin insights**.  
4. ✅ Dynamically handle DEV and PROD environments using GitHub Actions.

---

## **Tech Stack Used**  

| Component                      | Purpose                                                                                                                                       |  
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|  
| **Google Cloud Composer (Airflow)**  | Orchestrate the pipeline (file arrival → Spark job → BigQuery)                                                                                   |  
| **Google Cloud Dataproc (Serverless)** | Run PySpark jobs to transform and process the flight booking data.                                                                            |  
| **Google Cloud Storage (GCS)**       | Store raw CSV files, transformed data, and Spark job files.                                                                                    |  
| **Google Cloud BigQuery (BQ)**      | Store the final processed and aggregated data for business insights.                                                                           |  
| **GitHub Actions (CI/CD)**         | Automate deployment of DAGs and Spark jobs based on branch push (dev → DEV, main → PROD).                                                     |  
| **Python / PySpark**              | Data processing, transformation, and aggregation.                                                                                             |  

---

## **Data Pipeline Flow**  

Here’s how the data flows through the pipeline:  

1. **Data Ingestion:**  
   - A CSV file (`flight_booking.csv`) is uploaded to **Google Cloud Storage (GCS)**.  
   - Airflow DAG waits for the file using **GCSObjectExistenceSensor**.  

2. **Data Processing (Spark Job):**  
   - Once the file is detected, **Airflow triggers a Dataproc Serverless job**.  
   - The Spark job reads the data from GCS, performs transformations, and generates insights.  

3. **Data Storage (BigQuery):**  
   - Transformed data and insights are written to **BigQuery** in separate tables:  
     - ✅ **Transformed Table:** Contains cleaned and enriched flight booking data.  
     - ✅ **Route Insights Table:** Shows insights on popular routes, average flight duration, etc.  
     - ✅ **Origin Insights Table:** Provides insights based on the origin of the booking.  

4. **Deployment Automation (CI/CD):**  
   - Whenever you push code to:  
     - **dev branch → Deploys to DEV environment**.  
     - **main branch → Deploys to PROD environment**.  

---

## **BigQuery Tables Created**  

| Table Name                        | Description                                                                                         |  
|----------------------------------|----------------------------------------------------------------------------------------------------|  
| **transformed_table**              | Contains the cleaned and transformed flight booking data.                                           |  
| **route_insights_table**           | Aggregated insights showing the most popular flight routes, avg stay length, etc.                  |  
| **origin_insights_table**          | Insights based on the origin city of the booking.                                                  |  

---

## **Files Structure**  

The repository has the following structure:  

```
.
│
├── airflow_job/
│   ├── airflow_job.py                <-- Airflow DAG to orchestrate the pipeline
│
├── spark_job/
│   ├── spark_transformation_job.py    <-- PySpark script for data processing
│
├── variables/
│   ├── dev/variables.json            <-- Environment variables for DEV
│   ├── prod/variables.json           <-- Environment variables for PROD
│
├── .github/workflows/
│   ├── ci-cd.yaml                    <-- GitHub Actions for CI/CD deployment
│
├── README.md                         <-- This file
```

---

## **Environment Setup**  

### Step 1: Clone the Repository  
```bash
git clone <repository-url>
cd flight-booking-data-pipeline
```

### Step 2: Create Environment Variables in GitHub  
In your **GitHub repository**, go to:  
`Settings` → `Secrets and variables` → `Actions` → Add the following secrets:  

| Secret Name               | Description                                       |  
|--------------------------|---------------------------------------------------|  
| **GCP_SA_KEY**            | Service account key JSON from GCP.                |  
| **GCP_PROJECT_ID**        | Your GCP project ID.                              |  

---

## **Deployment Using CI/CD**  

The **CI/CD Pipeline (ci-cd.yaml)** is set up as:  

| Branch Push       | Deployment Environment       |  
|-------------------|-----------------------------|  
| **dev**           | Deploys to **DEV**           |  
| **main**          | Deploys to **PROD**          |  

---

## **Airflow DAG Overview**  

The **Airflow DAG** performs the following tasks:  

1. **Wait for File:** Uses `GCSObjectExistenceSensor` to detect when the file arrives.  
2. **Trigger Spark Job:** Runs the Spark job in **Dataproc Serverless**.  
3. **Load to BigQuery:** Pushes the transformed data to BigQuery tables.  

---

## **Data Processing Flow in Spark Job**  

The **Spark Job** (`spark_transformation_job.py`) does the following:  

1. ✅ **Read Data from GCS.**  
2. ✅ **Transform Data:**  
   - Add **is_weekend** column.  
   - Categorize lead time into **Last-Minute, Short-Term, Long-Term**.  
   - Calculate **booking_success_rate**.  
3. ✅ **Generate Insights:**  
   - Route Insights (top routes, avg stay, etc.)  
   - Origin Insights (origin success rate, etc.)  
4. ✅ **Write Data to BigQuery.**

---

## **How to Test the Pipeline**  

### **Upload CSV File to GCS**  
Upload a sample CSV file to the GCS bucket:  
```
gs://airflow-projects-kv/airflow-project-1/source-dev/flight_booking.csv
```

### **Trigger the DAG**  
In **Airflow UI**, trigger the DAG manually.  

### **Check BigQuery Tables**  
Run queries in BigQuery:  
```sql
SELECT * FROM `proven-record-452706-g5.flight_data_dev.transformed_table`;
SELECT * FROM `proven-record-452706-g5.flight_data_dev.route_insights_table`;
SELECT * FROM `proven-record-452706-g5.flight_data_dev.origin_insights_table`;
```

---

## **Troubleshooting**  

| Error                                              | Solution                                               |  
|---------------------------------------------------|--------------------------------------------------------|  
| **Airflow DAG not running**                      | Check if file path matches the GCS bucket.            |  
| **Spark Job failed**                             | View logs in Dataproc > Serverless > Job logs.        |  
| **BigQuery table empty**                         | Verify that Spark job completed without errors.       |  

---

## **Screenshots Section**  

Here are the screenshots you should add under the `/images` folder:  

| Screenshot Number | Screenshot Path | Description |  
|-------------------|-----------------|-------------|  
| **1**             | `/images/screenshot1.jpg` | Github push to dev branch. |  
| **2**             | `/images/screenshot2.jpg` | dev CICD job pipeline. |  
| **3**             | `/images/screenshot3.jpg` | Spark job file uploaded to GCS bucket. |  
| **4**             | `/images/screenshot4.jpg` | variables.json uploaded to airflow bucket in GCP. |  
| **5**             | `/images/screenshot5.jpg` | flight booking DAG queued in airflow dev. |  
| **6**             | `/images/screenshot6.jpg` | Variables updated in airflow dev admin, Airflow Variables showing environment configs. |  
| **7**             | `/images/screenshot7.jpg` | Flight booking DAG graph in airflow dev. |  
| **8**             | `/images/screenshot8.jpg` | Spark job in Flight booking DAG generating batch process in Dataproc. |  
| **9**             | `/images/screenshot9.jpg` | Dataproc batch process logs|  
| **10**            | `/images/screenshot10.jpg` | Transformed data uploaded to dev Bigquery dataset. |  
| **11**            | `/images/screenshot11.jpg` | GitHub main and dev branches merged. |  
| **12**            | `/images/screenshot12.jpg` | upload to Prod CICD job pipeline. |  
| **13**            | `/images/screenshot13.jpg` | Airflow job uploaded to prod Bucket. |  
| **14**            | `/images/screenshot14.jpg` | Flight booking airflow DAG. |  
| **15**            | `/images/screenshot15.jpg` | .csv data uploaded to GCS bucket. |  
| **16**            | `/images/screenshot16.jpg` | Prod dataproc batch process generated. |  
| **17**            | `/images/screenshot17.jpg` | Prod dataproc batch process logs. |  
| **18**            | `/images/screenshot18.jpg` | Transformed data uploaded to prod Bigquery dataset. |  


---

## **Conclusion**  

This project demonstrates a full-fledged **data engineering pipeline** for processing flight booking data with:  
- ✅ **CI/CD Deployment.**  
- ✅ **Airflow Orchestration.**  
- ✅ **Serverless Spark Jobs.**  
- ✅ **BigQuery Insights.**  

---
###  **Future Enhancements to Scale and Optimize the Flight Booking Data Pipeline** 


### **1. Implement Real-time Streaming with Kafka** 🚀  
- Replace the current batch processing with **real-time streaming** using **Kafka** or **Google Pub/Sub**.  
- Stream live flight booking data, process it in **Spark Structured Streaming**, and push it to BigQuery.  
- This will simulate real-world real-time booking systems like airline reservations.  

---

### **2. Implement Delta Lake (Lakehouse Architecture)** 💎  
- Use **Delta Lake** on **Databricks** to implement a **data lakehouse**.  
- Ingest raw data into **Bronze layer**, transform data in **Silver layer**, and push insights to **Gold layer**.  
- This approach will improve data governance, ACID transactions, and time-travel support.  

---

### **3. Implement Slowly Changing Dimensions (SCD) in BigQuery** ⏳  
- Modify the Spark job to implement **SCD Type 2** in BigQuery.  
- Track historical changes (like booking modifications or cancellations) instead of overwriting data.  
- This is useful for maintaining data history and tracking customer behavior.  

---

### **4. Add Data Quality Checks Using Great Expectations** 📊  
- Integrate **Great Expectations (GE)** to validate incoming data in Spark before writing to BigQuery.  
- Check for missing values, invalid dates, and null bookings to ensure data quality.  
- Fail the pipeline if data quality doesn't meet set thresholds.  

---

### **5. Automate Data Lineage and Metadata Management** 🗺️  
- Use **Google Data Catalog** or **OpenMetadata** to capture data lineage from source to destination.  
- Track which data came from GCS, how Spark transformed it, and where it landed in BigQuery.  
- This will make it easier to trace issues and understand data flow.  

---

### **6. Add Monitoring and Alerting with Datadog/Grafana** 📈  
- Integrate **Datadog** or **Prometheus + Grafana** to monitor Spark job performance and Airflow DAG runs.  
- Set up alerts if the Spark job fails, Airflow tasks are delayed, or data anomalies occur.  
- This will help you monitor pipeline health in real-time.  

---

### **7. Use Infrastructure as Code (IaC) with Terraform** 🏗  
- Automate the provisioning of GCS buckets, Dataproc clusters, BigQuery tables, and Airflow Composer using **Terraform**.  
- This will enable easy replication of environments (Dev, QA, Prod) without manual setup.  
- It ensures infrastructure is version-controlled and reproducible.  

---

### **8. Deploy Spark Jobs Using KubernetesPodOperator** 🚀  
- Migrate from **Dataproc Serverless** to **KubernetesPodOperator** on GKE (Google Kubernetes Engine).  
- This will give more control over resource allocation and scaling.  
- You can also reduce costs by managing cluster resources efficiently.  

---

### **9. Integrate Power BI / Looker for Reporting** 📊  
- Connect BigQuery tables to **Power BI** or **Looker** for interactive dashboards.  
- Build insights like booking success rate, popular routes, and origin patterns.  
- This will provide clear business intelligence to stakeholders.  

---

### **10. Implement Data Lakehouse with Hudi/Iceberg** 💎  
- Replace direct GCS-to-BigQuery processing with a **Data Lakehouse** using **Apache Hudi** or **Iceberg**.  
- This will enable incremental data loads, ACID transactions, and time travel.  
- It also reduces data duplication and improves performance.  

---




