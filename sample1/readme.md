# Docker
## Create and test image

    docker build -t letsencrypt2 .
    docker run -p 8081:80 letsencrypt2

# Google Cloud Platform
## Login

    gcloud auth login
    gcloud auth configure-docker

## Tag and Push images

    docker tag letsencrypt1 eu.gcr.io/$PROJECT/letsencrypt1
    docker push eu.gcr.io/$PROJECT/letsencrypt1

## Create cluster

    gcloud config set compute/zone europe-west6-c
    gcloud container clusters create letsencrypt-test1
    gcloud config set container/use_application_default_credentials true
    gcloud auth application-default login
    gcloud container clusters get-credentials letsencrypt-test1

## Run container

    kubectl run letsencrypt1 --image eu.gcr.io/$PROJECT/letsencrypt1 --port 80


## Expose container

    kubectl expose deployment letsencrypt1 --type LoadBalancer --port 80 --target-port 80
    kubectl get service letsencrypt1

