# Kubernetes

This repo will deploy three 'microservices'

1) ReactJS front end

* docker run -d -p 80:80 boweihan/sentiment-analysis-frontend

2) Java web application

* docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://<container_ip or docker machine ip>:5000' boweihan/sa-webapp

3) Python sentiment analysis application

* docker run -d -p 5000:5000 boweihan/sa-logic


### Running in minikube

```
minikube start
mini
