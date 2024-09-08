# MongoDB, Mongo Express & Monitoring with Prometheus and Grafana on Kubernetes

## Project Overview
This project sets up MongoDB and Mongo Express in a Kubernetes environment, providing a web interface to interact with MongoDB. Additionally, Prometheus and Grafana are integrated to monitor MongoDB and its associated services.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Kubernetes Resources](#kubernetes-resources)
- [Step-by-Step Deployment](#step-by-step-deployment)
- [Setting up Monitoring with Prometheus and Grafana](#setting-up-monitoring-with-prometheus-and-grafana)
- [Accessing Services](#accessing-services)
- [Troubleshooting](#troubleshooting)

## Prerequisites
Make sure you have the following installed:
- Kubernetes Cluster (Minikube or any other)
- `kubectl` CLI
- Helm (v3+)
- NGINX Ingress Controller (if using ingress)
- Docker

Ensure you have an active Kubernetes cluster running before proceeding.

## Kubernetes Resources
This project contains the following key Kubernetes manifest files:
1. **mongo-config.yaml**: ConfigMap for MongoDB service URL.
2. **mongo-secrets.yaml**: Secret for MongoDB credentials (base64 encoded).
3. **mongodb-deployment-service.yaml**: Deployment and Service for MongoDB.
4. **mongo_express-deployment-service.yaml**: Deployment and Service for Mongo Express.
5. **mongo-express-ingress.yaml**: Ingress resource for accessing Mongo Express through a hostname.
6. **mongodb-pv.yaml**: PersistentVolume and PersistentVolumeClaim for MongoDB storage.
7. **mongodb-exporter.yaml**: MongoDB exporter for Prometheus monitoring.
8. **prometheus-values.yaml**: Custom values for Prometheus setup (optional).

## Step-by-Step Deployment

### 1. Deploy MongoDB and Mongo Express
Start by applying the Kubernetes manifests for MongoDB and Mongo Express.

```bash
kubectl apply -f mongo-config.yaml
kubectl apply -f mongo-secrets.yaml
kubectl apply -f mongodb-pv.yaml
kubectl apply -f mongodb-deployment-service.yaml
kubectl apply -f mongo_express-deployment-service.yaml
kubectl apply -f mongo-express-ingress.yaml
```

###############################################################################################################################################################
# Mongodb-and-express-monitoring-
MongoDb-and-MongoExpress deployment using Kubernetes with PV and PVC and adding Prometheus and Grafana setup in Minikube for monitor our Kubernetes Cluster

### Ensure that your Kubernetes cluster has an ingress controller installed to handle Ingress resources
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace
```


## Prometheus and Grafana setup in Minikube using Helm
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm repo list
kubectl create namespace monitoring
helm install prometheus prometheus-community/prometheus --namespace monitoring
helm install grafana grafana/grafana --namespace monitoring
```

### To expose the prometheus service
```bash
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np
```

### This will open a browser window with the Prometheus web interface.
```bash
minikube service prometheus-server-np
```
### To expose the Grafana service
```bash
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-np
```

### This will open a browser window with the Grafana web interface.
```bash
minikube service grafana-np
```

### To get the Grafana admin password, run the following:
```bash
echo "The Grafana admin password is:" && kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```








## Flow Chart Diagram

          (Start)
            |
            v
    (Kubernetes Cluster Initialization)
            |
            v
    (MongoDB Deployment)
            |
            v
    (Mongo Express Deployment)
            |
            v
     (MongoDB Exporter Deployment)
            |
            v
    (Set Up Prometheus & Grafana)
            |
            v
    (Access and Monitor Services)
            |
            v
          (End)
