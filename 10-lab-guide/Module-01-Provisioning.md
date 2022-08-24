# About

This module covers environment provisioning for the workshop.

### 1. Create a directory in Cloud Shell for the workshop
```
cd ~
mkdir gcp-spark-mllib-workshop
```

### 2. Clone the workshop git repo
```
cd ~/gcp-spark-mllib-workshop
git clone https://github.com/anagha-google/s8s-spark-mlops.git
```

### 3. Navigate to the Terraform provisioning directory
```
cd s8s-spark-mlops/00-env-setup
```

### 4. Provision the environment

#### 4.1. Define variables for use
Modify the below as appropriate for your deployment..e.g. region, zone etc.
```
PROJECT_ID=`gcloud config list --format "value(core.project)" 2>/dev/null`
PROJECT_NBR=`gcloud projects describe $PROJECT_ID | grep projectNumber | cut -d':' -f2 |  tr -d "'" | xargs`
PROJECT_NAME=`gcloud projects describe ${PROJECT_ID} | grep name | cut -d':' -f2 | xargs`
GCP_ACCOUNT_NAME=`gcloud auth list --filter=status:ACTIVE --format="value(account)"`
ORG_ID=`gcloud organizations list --format="value(name)"`
LOCATION=us-central1
VPC_NM=s8s-vpc-$PROJECT_NBR
SPARK_SERVERLESS_SUBNET=spark-snet
PERSISTENT_HISTORY_SERVER_NM=s8s-sphs-${PROJECT_NBR}
UMSA_FQN=s8s-lab-sa@$PROJECT_ID.iam.gserviceaccount.com
DATA_BUCKET=s8s_data_bucket-${PROJECT_NBR}
CODE_BUCKET=s8s_code_bucket-${PROJECT_NBR}
MODEL_BUCKET=s8s_model_bucket-${PROJECT_NBR}
YOUR_GCP_REGION="us-central1"
YOUR_GCP_ZONE="us-central1-a"
YOUR_GCP_MULTI_REGION="US"
BQ_CONNECTOR_JAR_GCS_URI="gs://spark-lib/bigquery/spark-bigquery-with-dependencies_2.12-0.22.2.jar"
CLOUD_COMPOSER_IMG_VERSION="composer-2.0.11-airflow-2.2.3"
SPARK_CUSTOM_CONTAINER_IMAGE_TAG="1.0.0"
CLOUD_SCHEDULER_TIME_ZONE="America/Chicago"

echo "PROJECT_ID=$PROJECT_ID"
echo "PROJECT_NBR=$PROJECT_NBR"
echo "PROJECT_NAME=$PROJECT_NAME"
echo "VPC_NM=$VPC_NM"
echo "PERSISTENT_HISTORY_SERVER_NM=$PERSISTENT_HISTORY_SERVER_NM"
echo "UMSA_FQN=$UMSA_FQN"
echo "DATA_BUCKET=$DATA_BUCKET"
echo "CODE_BUCKET=$CODE_BUCKET"
```

### 4.2. Initialize Terraform
Needs to run in cloud shell from ~/gcp-spark-mllib-workshop/s8s-spark-mlops/00-env-setup
```
terraform init
```

#### 4.3. Review the Terraform deployment plan
Needs to run in cloud shell from ~/gcp-spark-mllib-workshop/s8s-spark-mlops/00-env-setup
```
terraform plan \
  -var="project_id=${PROJECT_ID}" \
  -var="project_name=${PROJECT_NAME}" \
  -var="project_number=${PROJECT_NBR}" \
  -var="gcp_account_name=${GCP_ACCOUNT_NAME}" \
  -var="org_id=${ORG_ID}" \
  -var="cloud_composer_image_version=${CLOUD_COMPOSER_IMG_VERSION}" \
  -var="spark_container_image_tag=${SPARK_CUSTOM_CONTAINER_IMAGE_TAG}" \
  -var="gcp_region=${YOUR_GCP_REGION}" \
  -var="gcp_zone=${YOUR_GCP_ZONE}" \
  -var="gcp_multi_region=${YOUR_GCP_MULTI_REGION}" \
  -var="bq_connector_jar_gcs_uri=${BQ_CONNECTOR_JAR_GCS_URI}" \
  -var="cloud_scheduler_time_zone=${CLOUD_SCHEDULER_TIME_ZONE}"
```

#### 4.4. Provision the environment
Needs to run in cloud shell from ~/gcp-spark-mllib-workshop/s8s-spark-mlops/00-env-setup
```
cd ~/s8s-spark-mlops/00-env-setup/
terraform apply \
  -var="project_id=${PROJECT_ID}" \
  -var="project_name=${PROJECT_NAME}" \
  -var="project_number=${PROJECT_NBR}" \
  -var="gcp_account_name=${GCP_ACCOUNT_NAME}" \
  -var="org_id=${ORG_ID}"  \
  -var="cloud_composer_image_version=${CLOUD_COMPOSER_IMG_VERSION}" \
  -var="spark_container_image_tag=${SPARK_CUSTOM_CONTAINER_IMAGE_TAG}" \
  -var="gcp_region=${YOUR_GCP_REGION}" \
  -var="gcp_zone=${YOUR_GCP_ZONE}" \
  -var="gcp_multi_region=${YOUR_GCP_MULTI_REGION}" \
  -var="bq_connector_jar_gcs_uri=${BQ_CONNECTOR_JAR_GCS_URI}" \
  -var="cloud_scheduler_time_zone=${CLOUD_SCHEDULER_TIME_ZONE}" \
  --auto-approve
```

#### 4.5. For selective replacement of specific services/units of deployment
Needs to run in cloud shell from ~/gcp-spark-mllib-workshop/s8s-spark-mlops/00-env-setup
```
terraform apply -target=null_resource.vai_pipeline_customization \
-var="project_id=${PROJECT_ID}" \
  -var="project_name=${PROJECT_NAME}" \
  -var="project_number=${PROJECT_NBR}" \
  -var="gcp_account_name=${GCP_ACCOUNT_NAME}" \
  -var="org_id=${ORG_ID}"  \
  -var="cloud_composer_image_version=${CLOUD_COMPOSER_IMG_VERSION}" \
  -var="spark_container_image_tag=${SPARK_CUSTOM_CONTAINER_IMAGE_TAG}" \
  -var="gcp_region=${YOUR_GCP_REGION}" \
  -var="gcp_zone=${YOUR_GCP_ZONE}" \
  -var="gcp_multi_region=${YOUR_GCP_MULTI_REGION}" \
  -var="bq_connector_jar_gcs_uri=${BQ_CONNECTOR_JAR_GCS_URI}" \
  -var="cloud_scheduler_time_zone=${CLOUD_SCHEDULER_TIME_ZONE}" \
  --auto-approve
```

#### 4.6. To destroy the deployment
You can (a) shutdown the project altogether in GCP Cloud Console or (b) use Terraform to destroy. Use (b) at your own risk as its a little glitchy while (a) is guaranteed to stop the billing meter pronto.
<br>
Needs to run in cloud shell from ~/gcp-spark-mllib-workshop/s8s-spark-mlops/00-env-setup
```
cd ~/s8s-spark-mlops/00-env-setup/
terraform destroy \
  -var="project_id=${PROJECT_ID}" \
  -var="project_name=${PROJECT_NAME}" \
  -var="project_number=${PROJECT_NBR}" \
  -var="gcp_account_name=${GCP_ACCOUNT_NAME}" \
  -var="org_id=${ORG_ID}" \
  -var="cloud_composer_image_version=${CLOUD_COMPOSER_IMG_VERSION}" \
  -var="spark_container_image_tag=${SPARK_CUSTOM_CONTAINER_IMAGE_TAG}" \
  -var="gcp_region=${YOUR_GCP_REGION}" \
  -var="gcp_zone=${YOUR_GCP_ZONE}" \
  -var="gcp_multi_region=${YOUR_GCP_MULTI_REGION}" \
  -var="bq_connector_jar_gcs_uri=${BQ_CONNECTOR_JAR_GCS_URI}" \
  -var="cloud_scheduler_time_zone=${CLOUD_SCHEDULER_TIME_ZONE}" \
  --auto-approve
  ```