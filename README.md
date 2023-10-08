# Logistic-data-pipeline-using-HIVE-and-Airflow

![blueprint](https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/18d3df6d-857a-47ed-9c71-28867f8f085a)

# PROBLEM STATEMENT:

1) We will receive a csv file daily, its needs to ingested into Hive table without any duplicate data.
2) This pipeline needs to be automated using Airflow.

# Tech Stack used:

1) GCP Storage Bucket
2) HIVE 
3) GCP Airflow Composer

# BUILDIND DATA PIPELINE:

# GOALS:

1) To create a Pipeline which is used to ingest daily logistic data from GCP Storage Bucket to HIVE Partition table.
2) Inside GCP bucket input_data, csv files will be received and it needs to be ingested into Hive partioned table without any duplicate Data.
3) So once a csv file is inserted into table, it must be archived in a different bucket (archive) to avoid duplication.
4) Since, its a daily process we need to automate this process using Airflow.

