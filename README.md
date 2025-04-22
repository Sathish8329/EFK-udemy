# Kubernetes Logging Mechanism using EFK (Elasticsearch, Fluentd, and Kibana)

## Prerequisites
- **Docker setup:** [Install Docker](https://docs.docker.com/engine/install/)
- **Kubectl installation:** [Install Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- **Helm setup:** [Install Helm](https://helm.sh/docs/intro/install/)

---

## Step 1: Create Namespace
```sh
kubectl create namespace efk-monitoring
```

## Step 2: Add Elastic Helm Repository
```sh
helm repo add elastic https://helm.elastic.co
helm repo update
```

## Step 3: Install Elasticsearch
Check CIS driver permission (AmazonEBSCSIDriverPolicy) (use any one of three given below)
```sh
 helm install elasticsearch elastic/elasticsearch --version 7.17.3 -n logging   --set replicas=2   --set volumeClaimTemplate.storageClassName=ebs-sc   --set volumeClaimTemplate.resources.requests.storage=30Gi
```
```sh
helm install elasticsearch elastic/elasticsearch --version 7.17.3 -n efk-monitoring --set replicas=1
```

## Step 4: Install Elasticsearch with Persistence Disabled (For Testing Only)
```sh
helm install elasticsearch elastic/elasticsearch --version 7.17.3 -n efk-monitoring --set persistence.enabled=false,replicas=1
```

## Step 5: Check the Installation
```sh
helm list -n efk-monitoring
kubectl get pods -n efk-monitoring
kubectl get svc -n efk-monitoring
```

## Step 6: Install Kibana
```sh
helm install kibana elastic/kibana --version 7.17.3 -n efk-monitoring
```

## Step 7: Verify Kibana Installation
```sh
kubectl get pods -n efk-monitoring
```

## Step 8: Apply Fluentd Configuration
```sh
kubectl apply -f ./fluentd-config-map.yaml
kubectl apply -f ./fluentd-rbac.yaml
```

## Step 9: Monitor Fluentd DaemonSet
```sh
kubectl get pods -n kube-system -w
```

## Step 10: Add Dapr Helm Repository (Optional)
```sh
helm repo add dapr https://dapr.github.io/helm-charts/
helm repo update
```

## Step 11: Install Dapr (Optional)
Install Dapr with JSON formatted logs enabled:
```sh
helm install dapr dapr/dapr --namespace efk-monitoring --set global.logAsJson=true
```

## Step 12: Port Forward Kibana
To access the Kibana web interface:
```sh
kubectl port-forward svc/kibana-kibana 5601 -n efk-monitoring
```

## Step 13: Deploy the Sample Application
```sh
kubectl apply -f k8s-app.yml
kubectl get pods
kubectl get svc
kubectl logs <pod-name>
kubectl port-forward svc/myapp 8080
```

---

## Delete All the Resources
```sh
kubectl delete -f k8s-app.yml
kubectl delete -f fluentd-config-map.yaml
kubectl delete -f fluentd-rbac.yaml
helm list -n efk-monitoring
helm uninstall elasticsearch -n efk-monitoring
helm uninstall kibana -n efk-monitoring
```

---

This guide helps you set up logging for your Kubernetes cluster using EFK (Elasticsearch, Fluentd, and Kibana) with optional Dapr integration.

