# Mongodb-and-express-monitoring-
MongoDb-and-MongoExpress deployment using Kubernetes with PV and PVC and adding Prometheus and Grafana setup in Minikube for monitor our Kubernetes Cluster

### Ensure that your Kubernetes cluster has an ingress controller installed to handle Ingress resources
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace



Prometheus and Grafana setup in Minikube

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

# To get the Grafana admin password, run the following:
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode


### Final Helm Commands 

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

kubectl create namespace monitoring

helm install prometheus prometheus-community/prometheus --namespace monitoring
helm install grafana grafana/grafana --namespace monitoring

kubectl port-forward svc/grafana 3000:80 -n monitoring

# Optional MongoDB Exporter
helm install mongodb-exporter prometheus-community/prometheus-mongodb-exporter --namespace monitoring
