# SSL certificate
## Docs

 - [docs](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04)


## Generate certificate

    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./nginx/nginx-selfsigned.key -out ./nginx/nginx-selfsigned.crt
    sudo openssl dhparam -out ./nginx/dhparam.pem 2048

# Docker
## Create and test image

    docker build -t eu.gcr.io/$PROJECT/letsencrypt2 .
    docker run -p 8081:80 -p 8082:443 eu.gcr.io/$PROJECT/letsencrypt2

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

    kubectl run letsencrypt2 --image eu.gcr.io/$PROJECT/letsencrypt2 --port 80 --port 443


## Expose container

    kubectl expose deployment letsencrypt2 --name http --type LoadBalancer --port 80 --target-port 80
    kubectl expose deployment letsencrypt2 --name https --type LoadBalancer --port 443 --target-port 443
    kubectl get service letsencrypt2

