# Observability in Kubernetes

This repository contains the code for the presentation "Observability in Kubernetes" given at CNCF Community Group Santa Catarina Channel https://www.youtube.com/watch?v=TzeNt2qxIMM

## Tools needed

- k3d
- kubectl
- helm

## How to create your cluster

```sh

# Helm repositories
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update

# Create a k3d cluster with 3 nodes used to create our infra
k3d cluster create observability-example --image=rancher/k3s:v1.30.2-k3s1 --servers=3

# Namespaces used to install services
kubectl create namespace collectors
kubectl create namespace monitoring

# Install Prometheus, Jaeger, Loki and Grafana
helm install prometheus prometheus-community/prometheus --namespace=monitoring --values=./helm/prometheus.yaml
helm install jaeger jaegertracing/jaeger --namespace=monitoring --values=./helm/jaeger.yaml
helm install loki grafana/loki --namespace=monitoring --values=./helm/loki.yaml
helm install grafana grafana/grafana --namespace=monitoring --values=./helm/grafana.yaml

# Install OTel Collector
helm install otel-collector open-telemetry/opentelemetry-collector --namespace=collectors --values=./helm/otel-collector.yaml

```