# Prometheus Setup (For Study Purposes)

Make sure you have the following tools installed:

- [Docker](https://docs.docker.com/engine/install/): Container runtime required by K3d
- [k3d](https://k3d.io/stable/): Runs a lightweight Kubernetes cluster inside Docker
- [Helm](https://helm.sh/docs/intro/install/): Kubernetes package manager

---

## Create a K3d Cluster

Disable Traefik (the default ingress controller) to keep the environment minimal:

```bash
k3d cluster create --k3s-arg "--disable=traefik@server:0"
```

---

## Add and Update Helm Repositories

Add the Bitnami charts repository and update local Helm repo cache:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install prometheus prometheus-community/prometheus \
  --namespace monitoring --create-namespace \
  --set kube-state-metrics.image.registry=docker.io \
  --set kube-state-metrics.image.repository=bitnami/kube-state-metrics \
  --set kube-state-metrics.image.tag=2.10.1

```

---

### Prometheus

Port-forward the Prometheus service:

```bash
kubectl port-forward -n monitoring service/prometheus-server 9090:80
```

Open [http://localhost:9090](http://localhost:9090) in your browser.

---

---

### Node Exporter

Port-forward the Node Exporter service:

```bash
kubectl port-forward -n monitoring service/prometheus-prometheus-node-exporter 9091:9100
```

Open [http://localhost:9091](http://localhost:9091) in your browser.

---

### Alertmanager

Port-forward the Alertmanager service:

```bash
kubectl port-forward -n monitoring service/prometheus-alertmanager 9093:9093
```

Open [http://localhost:9093](http://localhost:9093) in your browser.

---

## To Uninstall Everything

```bash
helm uninstall prometheus -n monitoring
k3d cluster delete
```

---
