# Logistic-data-pipeline-using-HIVE-and-Airflow

![blueprint](https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/18d3df6d-857a-47ed-9c71-28867f8f085a)

# PROBLEM STATEMENT:

1) We will receive a csv file daily, its needs to ingested into Hive table without any duplicate data.
2) This pipeline needs to be automated using Airflow.

# Tech Stack used:

1) GCP Storage Bucket
2) HIVE 
3) Airflow Cloud Composer

# GOALS:

1) To create a Pipeline which is used to ingest daily logistic data from GCP Storage Bucket to HIVE Partition table.
2) Inside input_data bucket, we need to check whether file is received or not and it needs to be ingested into Hive partioned table without any duplicate Data.
3) So once a csv file is received it should be inserted into table and it must be archived in a different bucket (archive) to avoid duplication.
4) Since, its a daily process we need to automate this process using Airflow.

# BUILDIND DATA PIPELINE:

Dataset: 

HIVE_LOAD_AIRFLOW.PY:

This python script defines an Apache Airflow DAG for a data pipeline that loads logistics data into Hive on Google Cloud Dataproc. Let's break down the key components and tasks in this script:

Import Statements:

These lines import various modules and classes from Apache Airflow and Google Cloud libraries.

<img width="588" alt="image" src="https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/7e25e644-d1c4-4237-8ec0-b88ce3c05c5e">

1) GCSObjectsWithPrefixExistenceSensor --> we are using this Sensor is used to detect the existence of files in a Google Cloud Storage (GCS) bucket with a specific prefix.
3) DataprocSubmitHiveJobOperator --> This is an Airflow operator for submitting Hive jobs on Google Cloud Dataproc clusters.
4) days_ago --> This Airflow utility function used for calculating a date that is a specified number of days in the past.

CODE EXPLANATION:

Default Arguments and DAG Definition:

Setting up the default arguments and interval time. We can change the interval time start date according to our needs.

<img width="588" alt="image" src="https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/7bfd9979-873e-4f64-8ef7-b3c0a4ec5f93">

1) Here I'm creating "default_args" with default arguments and interval time.
2) Creating an instance of a DAG named 'hive_load_airflow_dag' with various parameters, including the default arguments, description, schedule interval, and start date.

DAG TASKS:
Sensing new file inside our bucket:

<img width="588" alt="image" src="https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/9922859b-9f5d-45bd-ae18-08bc9c437c0d">

This task uses the "GCSObjectsWithPrefixExistenceSensor" sensor to sense the existence of new files under input_data bucket to find files like 'logistics_%'  using the prefix parameter 'input_data/logistics_'.

CREATING DATABASE AND HIVE EXTERNAL TABLE:

<img width="637" alt="image" src="https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/035151e4-8500-4c08-8633-489c412e879f">

1) Once a new file is sensed, its need to be ingested into HIVE Table.
2) So this task will be used in Creating HIVE DB named 'logistics_db'and external table 'logistics_data' if not exists in GCP Dataproc cluster, which will fetch data from 'gs://logistic-raw-data/input_data/' location.

<img width="659" alt="image" src="https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/17f3503a-800e-4e52-9933-579ca9a46a57">

CREATING HIVE PARTIONED TABLE AND LOADING DATA INTO TABLE:

1) This task will create a HIVE table 'logistics_data_partitioned 'which is partioned by 'Date'.
2) Once table is created, we're setting the necessary for dynamic partioned and loading data from from previously created external table to our partioned table.

MOVING FILES TO ARCHIVE BUCKET AND DEFINING TASK DEPENDENCIES:

<img width="672" alt="image" src="https://github.com/balajide19/logistic-data-pipeline-using-HIVE-and-Airflow/assets/146630003/fe430c7e-b321-43d2-8d1f-aa3222dded5e">

1) This task will move the logistic data file which we received under input_data bucket to 'archive' bucket for avoiding duplicate data ingestion.
2) Tasks are executed sequentially by defining the task dependencies in the order as "sense_logistics_file >> create_hive_database >> create_hive_table >> create_partitioned_table >> set_hive_properties_and_load_partitioned >> archive_processed_file".








