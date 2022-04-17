## DESCRIPTION

Prometheus/Grafana Monitoring for MongoDB, using Helm and prometheus operator in order to create yaml files.

## PREREQUISITES

Docker, kubernetes, minikube must already be installed, as well as Helm.

## INSTALL PROMETHEUS-OPERATOR

```
minikube start --cpus 4 8192 --driver=docker
```

Find repo at https://artifacthub.io

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://charts.helm.sh/stable
helm repo update
```

## INSTALL CHART

```
helm install prometheus prometheus-community/kube-prometheus-stack
helm ls
```
---
## KUBECTL COMMAND

! Yaml files are only for documentation. New yaml files will be created, if applied.

```
kubectl get pod
kubectl get all
kubectl get configmap
kubectl get secret
kubectl get crd
kubectl get statefulset
kubectl describe statefulset prometheus-prometheus-kube-prometheus-prometheus > prom.yaml
kubectl describe statefulset alertmanager-prometheus-kube-prometheus-alertmanager > alert.yaml
kubectl get deployment
kubectl describe deployment prometheus-kube-prometheus-operator > oper.yaml
kubectl get secret prometheus-prometheus-kube-prometheus-prometheus -o yaml > secret.yaml
kubectl get configmap prometheus-prometheus-kube-prometheus-prometheus-rulefiles-0 -o yaml > config.yaml
```

## ACCESS GRAFANA UI 

```
kubectl get service
kubectl get deployment
kubectl port-forward prometheus-grafana
kubectl get pod
kubectl logs [grafana pod name] -c grafana
kubectl port-forward deployment/prometheus-grafana 3000
```

Monitoring information can be found at localhost:3000

Default Grafana Credentials

```
Username: admin
Password: prom-operator
```

## ACCESS PROMETHEUS UI

```
kubectl get pod
kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090
```
Service Monitor can be found at localhost:9090

---
---

## SET UP SERVICE MONITOR

```
kubectl get servicemonitor
kubectl get servicemonitor prometheus-grafana -o yaml
kubectl get crd
kubectl get prometheuses.monitoring.coreos.com -o yaml
```

## SET UP MONGODB AND SERVICE

```
kubectl apply -f mongodb.yaml
kubectl get pod
```

## PROMETHEUS EXPORTER

- The exporter communicates between app data and Prometheus metrics by converting data to correct format and exposes those metrics for use.

- Find mongodb-exporter at https://hub.docker.com/search?q=mongodb-exporter or at https://prometheus.io/docs/instrumenting/exporters/

- Add a MongoDB Helm Chart from https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-mongodb-exporter


- Finally, we need to overwrite the parameters of the values in the mongodb-exporter.yaml, before installing mongodb-exporter.yaml

! New exporter-values.yaml file will be created, if applied in this project.

```
helm show values [chart name]
helm show values prometheus-community/prometheus-mongodb-exporter > exporter-values.yaml
helm install mongodb-exporter prometheus-community/prometheus-mongodb-exporter -f exporter-values.yaml
helm ls
kubectl get pod
kubectl get service
kubectl get servicemonitor
kubectl port-forward service/mongodb-exporter-prometheus-mongodb-exporter 9216
kubectl get deployment
kubectl port-forward deployment/prometheus-grafana 3000
```