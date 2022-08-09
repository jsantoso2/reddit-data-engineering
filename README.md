# reddit-data-engineering of r/FedEx

- Create Kafka cluster using docker-compose hosted on GCP VM
- Used PRAW API to stream both comments and posts data from r/FedEx into Kafka
- Consumes streaming data from Kafka using Spark Streaming
- Saves data into Google Cloud Storage Bucket (Data Lake) -> Batch load data into BigQuery (Data Warehouse) using Cloud Functions
- Visualization of Data in Google Data Studio

### Purpose of this project:
- Learn technologies like (Kafka, Spark Streaming, Data Studio), NOT producing the best/optimal architecture

### Dashboard Link: https://datastudio.google.com/reporting/afbed74b-8f18-4012-bc91-ddbc66e4574c

### Pipeline Diagram:

Currently, Spark Stream writes a new file every 4 hours -> which triggers the Cloud Function to load to BigQuery 

### Tools/Framework Used:
- Terraform: To create and standardize GCP resources (ex: storage bucket, VM, dataproc cluster, etc.)
- Docker: Used docker compose to create Kafka cluster hosted on GCP VM
- Python + PRAW: Extract data from reddit
- Kafka: Message broker to handle stream data
- Spark Streaming: Consumer of Streaming data from Kafka
- Google Cloud Storage Bucket: Data Lake to store files produced by Spark Streaming
- Google Cloud Function: To batch load data from GCS to BigQuery because streaming insets are expensive!
- Google BigQuery: Data Warehouse to store data so that it can be queried
- Google Data Studio: Visualization Tool to create dashboard

### Procedure/General Setup
1. Create resources with Terraform
2. Kafka
    - Reserve Static External IP address for Kafka VM
    - Install Dependencies in Kafka VM (Docker, PRAW python library)
    - Set "KAFKA_ADDRESS" env variable to exeternal IP of Kafka VM
    - Utilize "screen" command to run docker compose command to start Kafka cluster
    - Utilize "screen" command to run both python producer programs
3. Spark Streaming
    - Use SparkStreaming.py file to submit PySpark job to cluster
4. Cloud Functions
    - Use "Create/Finializing" Trigger event in GCS bucket to execute Cloud Function to upload new Parquet file to BigQuery
5. Data Studio
    - Connect to BigQuery

