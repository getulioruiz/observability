# 📊 Prometheus Stack Setup (For Study Purposes)

This guide helps you install and run the Prometheus monitoring stack — including Grafana, Alertmanager, and exporters — on a lightweight K3d Kubernetes cluster, suitable for local development or study labs.

---

## ✅ Prerequisites

Make sure you have the following tools installed:

- [Docker](https://docs.docker.com/engine/install/): Container runtime required by K3d
- [k3d](https://k3d.io/stable/): Runs a lightweight Kubernetes cluster inside Docker
- [Helm](https://helm.sh/docs/intro/install/): Kubernetes package manager

---

## 🚀 Create a K3d Cluster

Disable Traefik (the default ingress controller) to keep the environment minimal:

```bash
k3d cluster create --k3s-arg "--disable=traefik@server:0"
```

---

## 📦 Add and Update Helm Repositories

Add the Bitnami charts repository and update local Helm repo cache:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

---

## 📥 Install Prometheus Stack via Helm

Install the `kube-prometheus-stack`, configuring image sources and disabling admission webhooks (optional for local environments like k3d):

```bash
helm install prometheus-stack prometheus-community/kube-prometheus-stack \
  --namespace monitoring --create-namespace \
  --set kube-state-metrics.image.registry=docker.io \
  --set kube-state-metrics.image.repository=bitnami/kube-state-metrics \
  --set kube-state-metrics.image.tag=2.10.1 \
  --set prometheusOperator.admissionWebhooks.enabled=false
```

---

## 🌐 Accessing Dashboards

### Grafana

Port-forward the Grafana service:

```bash
kubectl port-forward -n monitoring svc/prometheus-stack-grafana 3000:80
```

Then open [http://localhost:3000](http://localhost:3000) in your browser.

**Credentials**:
- **Username**: `admin`
- **Password**: Run the following command to retrieve it:

  ```bash
  kubectl --namespace monitoring get secret prometheus-stack-grafana \
    -o jsonpath="{.data.admin-password}" | base64 -d && echo
  ```

---

### Prometheus

Port-forward the Prometheus service:

```bash
kubectl port-forward -n monitoring svc/prometheus-stack-kube-prom-prometheus 9090:9090
```

Open [http://localhost:9090](http://localhost:9090) in your browser.

---

### Alertmanager

Port-forward the Alertmanager service:

```bash
kubectl port-forward -n monitoring svc/prometheus-stack-kube-prom-alertmanager 9093:9093
```

Open [http://localhost:9093](http://localhost:9093) in your browser.

---

## 🧹 To Uninstall Everything

If you want to clean up the Prometheus stack and the cluster:

```bash
helm uninstall prometheus-stack -n monitoring
k3d cluster delete
```

---
