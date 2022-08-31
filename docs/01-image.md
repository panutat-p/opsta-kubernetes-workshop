### Prepare Bookinfo Docker Image on Google Cloud Container Registry (GCR)

* kubeconfig with create and full privilege control on namespaces
* 3 domains ready to point to bookinfo application for each environment. For example
    * Dev Environment: `bookinfo.dev.opsta.net`
    * UAT Environment: `bookinfo.uat.opsta.net`
    * Production Environment: `bookinfo.opsta.net`

* Enable GCR API
* Enable billing for this project

```bash
gcloud auth login

# CHANGE THESE VARIABLES
export K8S_NAME=skooldio-k8s
export K8S_ZONE=asia-southeast1-b
export PROJECT_ID=opsta-kubernetes-workshop

gcloud projects create $PROJECT_ID
gcloud config set project $PROJECT_ID

# Authentication with GCR
gcloud iam service-accounts create $K8S_NAME
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member "serviceAccount:$K8S_NAME@$PROJECT_ID.iam.gserviceaccount.com" \
  --role "roles/storage.admin"
gcloud iam service-accounts keys create keyfile.json --iam-account $K8S_NAME@$PROJECT_ID.iam.gserviceaccount.com
cat keyfile.json | docker login -u _json_key --password-stdin https://asia.gcr.io

gcloud services enable containerregistry.googleapis.com

# Pull all bookinfo micro-services
docker pull opsta/bookinfo-ratings:latest
docker pull opsta/bookinfo-productpage:latest
docker pull opsta/bookinfo-details:latest
docker pull opsta/bookinfo-reviews:latest

# Tag to your GCR account
docker tag opsta/bookinfo-ratings:latest asia.gcr.io/$PROJECT_ID/bookinfo-ratings:dev
docker tag opsta/bookinfo-ratings:latest asia.gcr.io/$PROJECT_ID/bookinfo-ratings:uat
docker tag opsta/bookinfo-ratings:latest asia.gcr.io/$PROJECT_ID/bookinfo-ratings:prd
docker tag opsta/bookinfo-productpage:latest asia.gcr.io/$PROJECT_ID/bookinfo-productpage:dev
docker tag opsta/bookinfo-productpage:latest asia.gcr.io/$PROJECT_ID/bookinfo-productpage:uat
docker tag opsta/bookinfo-productpage:latest asia.gcr.io/$PROJECT_ID/bookinfo-productpage:prd
docker tag opsta/bookinfo-details:latest asia.gcr.io/$PROJECT_ID/bookinfo-details:dev
docker tag opsta/bookinfo-details:latest asia.gcr.io/$PROJECT_ID/bookinfo-details:uat
docker tag opsta/bookinfo-details:latest asia.gcr.io/$PROJECT_ID/bookinfo-details:prd
docker tag opsta/bookinfo-reviews:latest asia.gcr.io/$PROJECT_ID/bookinfo-reviews:dev
docker tag opsta/bookinfo-reviews:latest asia.gcr.io/$PROJECT_ID/bookinfo-reviews:uat
docker tag opsta/bookinfo-reviews:latest asia.gcr.io/$PROJECT_ID/bookinfo-reviews:prd

# Push docker image back to your GCR account
docker push asia.gcr.io/$PROJECT_ID/bookinfo-ratings:dev
docker push asia.gcr.io/$PROJECT_ID/bookinfo-ratings:uat
docker push asia.gcr.io/$PROJECT_ID/bookinfo-ratings:prd
docker push asia.gcr.io/$PROJECT_ID/bookinfo-productpage:dev
docker push asia.gcr.io/$PROJECT_ID/bookinfo-productpage:uat
docker push asia.gcr.io/$PROJECT_ID/bookinfo-productpage:prd
docker push asia.gcr.io/$PROJECT_ID/bookinfo-details:dev
docker push asia.gcr.io/$PROJECT_ID/bookinfo-details:uat
docker push asia.gcr.io/$PROJECT_ID/bookinfo-details:prd
docker push asia.gcr.io/$PROJECT_ID/bookinfo-reviews:dev
docker push asia.gcr.io/$PROJECT_ID/bookinfo-reviews:uat
docker push asia.gcr.io/$PROJECT_ID/bookinfo-reviews:prd
```
