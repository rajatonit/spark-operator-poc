# spark-operator-poc

helm install spark-release ./charts/spark-operator-chart/ --namespace spark-operator --create-namespace
helm status spark-release --namespace spark-operator


kubectl create clusterrolebinding 121266199260-compute-cluster-admin-binding --clusterrole=cluster-admin --user=121266199260-compute@developer.gserviceaccount.com

kubectl create clusterrolebinding root-cluster-admin-binding --clusterrole=cluster-admin --user=root

kubectl get sparkapplications spark-pi -o=yaml -n spark-operator

# Instructions to deploy on GCP with History Server

### Deploy Spark Server

1. Create namespace
 kubectl create namespace spark-operator

2. Install helm file
helm install spark-release ./charts/spark-operator-chart/ --namespace spark-operator

3. Check Pods
kubectl get pods -n spark-operator

4. Hack for it to work on latest GCP
kubectl create clusterrolebinding 121266199260-compute-cluster-admin-binding --clusterrole=cluster-admin --user=121266199260-compute@developer.gserviceaccount.com

5.  Create Key Mount
kubectl create -n spark-operator secret generic google-sa-key \
--from-file google_sa_key.json \
--from-literal=path=/creds/google_sa_key.json

6. Apply Yaml Python example
 kubectl apply -f ./examples/spark-bucket-logging-ex.yaml

 7. Check if working
 kubectl get sparkapplications spark-pi -o=yaml -n spark-operator

8. Run History server
kubectl apply -f ./spark-history-server/deployment.yaml

9. View app
