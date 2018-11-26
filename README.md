# Kubernetes

This repo will deploy three 'microservices'

1) ReactJS front end

* docker run -d -p 80:80 boweihan/sentiment-analysis-frontend

2) Java web application

* docker run -d -p 8080:8080 -e SA_LOGIC_API_URL='http://<container_ip or docker machine ip>:5000' boweihan/sa-webapp

3) Python sentiment analysis application

* docker run -d -p 5000:5000 boweihan/sa-logic

### Contents

* Build / package and run ReactJS, Java and Python applications
* building and publishing docker containers with Dockerfiles to container registry
* Kubernetes - Pods, Services, Deployments, Zero-downtime deployments, Scaling
* Migrate microservice application to kubernetes cluster



### Running in minikube

```
minikube start

kubectl create -f resource-manifests/sa-frontend-pod.yaml
kubectl port-forward sa-frontend 80:80
kubectl get pods

DEPLOY WEB ON ANOTHER POD
sudo kubectl create -f resource-manifests/sa-frontend-pod2.yaml
kubectl get pods

CREATE SERVICE FOR FRONTENDS
kubectl create -f resource-manifests/service-sa-frontend-lb.yaml
kubectl get svc

TO RUN THE LOAD BALANCED APPLICATION
kubectl get svc

CREATE A DEPLOYMENT INSTEAD OF TWO SEPARATE PODS
kubectl apply -f resource-manifests/sa-frontend-deployment.yaml

DELETE THE OLD PODS
kubectl delete pod sa-frontend
kubectl delete pod sa-frontend2

*** IF YOU DELETE A DEPLOYMENT POD NOW IT WILL BE RECREATED ***

DEPLOY A NEW GREEN IMAGE (--record=true will create a history description)
kubectl apply -f resource-manifests/sa-frontend-deployment-green.yaml --record=true

CHECK DEPLOYMENT ROLLOUT
kubectl rollout status deployment sa-frontend

LIST DEPLOYMENT HISTORY
kubectl rollout history deployment sa-frontend

ROLLBACK TO REVISION 1
kubectl rollout undo deployment sa-frontend --to-revision=1

DEPLOY PYTHON SERVICE (labelled with sa-logic)
kubectl apply -f sa-logic-deployment.yaml --record

CREATE SERVICE THAT ACTS AS AN ENTRY POINT FOR PYTHON APP
kubectl apply -f service-sa-logic.yaml

*** SERVICES ARE EXPOSED VIA kube-dns so you can access via http://sa-logic ***

DEPLOY JAVA WEB APP
kubectl apply -f sa-web-app-deployment.yaml --record

CREATE SERVICE AS ENTRY POINT FOR JAVA APP
kubectl apply -f service-sa-web-app-lb.yaml

UPDATE THE FRONT END TO POINT TO JAVA SERVICE INSTEAD OF LOCALHOST:8000

minikube service list (get the IP of sa-web-app-lb)
modify App.js
npm run build
docker build -f Dockerfile -t $DOCKER_USER_ID/sentiment-analysis-frontend:minikube .
docker push $DOCKER_USER_ID/sentiment-analysis-frontend:minikube
kubectl apply -f sa-frontend-deployment.yaml

SERVE WITH NGROK TO THE PUBLIC
./ngrok http $DEPLOYMENT_IP:$DEPLOYMENT_PORT

```


