# About

The following are the services that get provisioned, and the code and configuration that gets uploaded into your environment. The screenshots present a view of the author's environment, yours should be identical.

[1. IAM](Services-Created.md#1-iam) <br>
[2. Networking](Services-Created.md#2-networking) <br>
[3. Cloud Storage](Services-Created.md#3-cloud-storage) <br>
[4. BigQuery](Services-Created.md#4-bigquery) <br>
[5. Persistent Spark History Server](Services-Created.md#5-persistent-spark-history-server) <br>
[6a. Vertex AI Workbench - User Managed Notebook Server ]() <br>
[6b. Vertex AI Workbench - User Managed Notebook Server - Jupyter Notebook]() <br>
[7a. Vertex AI Workbench - Managed Notebook Server ]() <br>
[7b. Vertex AI Workbench - Managed Notebook Server - Jupyter Notebooks]() <br>
[8a. Google Container Registry]() <br>
[9a. Cloud Composer]() <br>
[9b. Cloud Composer - Airflow variables]() <br>
[9c. Cloud Composer - Airflow DAG]() <br>
[10. Google Cloud Function]() <br>
[11. 	Cloud Scheduler]() <br>
[12. Customized Vertex AI pipeline JSON in GCS]() <br>

## 1. IAM
A User Managed Service Account (UMSA) is created and granted requisite permissions and the lab attendee is granted permissions to impersonate the UMSA. There are a few other permissions granted to the default Google Managed Service Accounts of some services as requried. The Terraform main.tf is a good read to understand the permissions.

![IAM](../06-images/module-1-iam-01.png)   
<br><br>

![IAM](../06-images/module-1-iam-02.png)   
<br><br>

![IAM](../06-images/module-1-iam-03.png)   
<br><br>

![IAM](../06-images/module-1-iam-04.png)   
<br><br>


## 2. Networking
The following networking components are created as part of Terraform deployment-

### 2.1. VPC

![VPC](../06-images/module-1-networking-01.png)   
<br><br>

![VPC](../06-images/module-1-networking-02.png)   
<br><br>

### 2.2. Subnet with private google access

![VPC](../06-images/module-1-networking-03.png)   
<br><br>

### 2.3. Firewall rule for Data Serverless Spark

![VPC](../06-images/module-1-networking-05.png)   
<br><br>


### 2.4. Reserved IP for VPC peering with Vertex AI tenant network for Vertex AI workbench, managed notebook instance for BYO network

![VPC](../06-images/module-1-networking-04.png)   
<br><br>


### 2.5. VPC peering with Vertex AI tenant network for Vertex AI workbench, managed notebook instance for BYO network

![VPC](../06-images/module-1-networking-06.png)   
<br><br>

![VPC](../06-images/module-1-networking-07.png)   
<br><br>

![VPC](../06-images/module-1-networking-08.png)   
<br><br>

## 3. Cloud Storage

### 3.1. Buckets created
A number of buckets are created by te Terraform and some buckets are created by the GCP products. The following is a listing of buckets created as part of the deployment with Terraform.

![GCS](../06-images/module-1-storage-01.png)   
<br><br>

![GCS](../06-images/module-1-storage-02.png)   
<br><br>

### 3.2. The Data Bucket
The following is the author's data bucket content-
```
customer_churn_score_data.csv
customer_churn_train_data.csv
```

### 3.3. The Code Bucket

The following is the author's code bucket content-
```
# Cloud Composer - Airflow DAG
airflow/pipeline.py

# Shell Script for building custom container image for Serverless Spark
bash/build-container-image.sh

# Post startup shell scripts to upload Jupyter notebooks in GCS to Vertex AI workbench notebook server instances
bash/mnbs-exec-post-startup.sh
bash/umnbs-exec-post-startup.sh

# Pyspark scripts for Spark Machine Learning
pyspark/batch_scoring.py
pyspark/common_utils.py
pyspark/hyperparameter_tuning.py
pyspark/model_training.py
pyspark/preprocessing.py

# Cloud Functions source code
cloud-functions/function-source.zip
cloud-functions/main.py
cloud-functions/requirements.txt
```

### 3.4. The Notebook Bucket

```
# PySpark development notebooks
pyspark/batch_scoring.ipynb
pyspark/hyperparameter_tuning.ipynb
pyspark/model_training.ipynb
pyspark/preprocessing.ipynb

# Vertex AI pipeline development notebook
vai-pipelines/customer_churn_training_pipeline.ipynb
```

### 3.5. The Pipeline Bucket

The customized (for your environment) JSON for scheduling a Vertex AI pipeline.
```
templates/customer_churn_vai_pipeline_template.json
```

### 3.6. The Functions Bucket

Cloud Functions source code
```
function-source.zip
```

### 3.6. The rest of the buckets
Are empty and used for peristing logs and/or MLOps artifacts



## 4. BigQuery

![BQ](../06-images/module-1-bq-01.png)   
<br><br>

![BQ](../06-images/module-1-bq-02.png)   
<br><br>

## 5. Persistent Spark History Server

![PHS](../06-images/module-1-phs-01.png)   
<br><br>

![PHS](../06-images/module-1-phs-02.png)   
<br><br>

![PHS](../06-images/module-1-phs-03.png)   
<br><br>

![PHS](../06-images/module-1-phs-04.png)   
<br><br>

![PHS](../06-images/module-1-phs-05.png)   
<br><br>

## 6a. Vertex AI Workbench - Managed Notebook Server 

![UMNBS](../06-images/module-1-vai-wb-01.png)   
<br><br>

![UMNBS](../06-images/module-1-vai-wb-mnb-01.png)   
<br><br>

## 6b. Vertex AI Workbench - Managed Notebook Server - Jupyter Notebook

![UMNBS](../06-images/module-1-vai-wb-mnbs-02.png)   
<br><br>

## 7a. Vertex AI Workbench - Managed Notebook Server 

![MNBS](../06-images/module-1-vai-wb-mnb-01.png)   
<br><br>

## 7b. Vertex AI Workbench - Managed Notebook Server - Jupyter Notebooks

## 8a. Google Container Registry

## 8b. Google Container Registry - Container Image

## 9a. Cloud Composer

## 9b. Cloud Composer - Airflow variables

## 9c. Cloud Composer - Airflow DAG

## 10. Google Cloud Function

## 11. 	Cloud Scheduler

## 12. Customized Vertex AI pipeline JSON in GCS