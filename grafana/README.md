# Grafana Setup (For Study Purposes)

Make sure you have the following tools installed and also your prometheus is up and running:

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
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana \
  --namespace monitoring \
  --set adminPassword='admin' \
  --set service.type=ClusterIP \
  --set 'datasources.datasources\.yaml.apiVersion=1' \
  --set 'datasources.datasources\.yaml.datasources[0].name=Prometheus' \
  --set 'datasources.datasources\.yaml.datasources[0].type=prometheus' \
  --set 'datasources.datasources\.yaml.datasources[0].url=http://prometheus-server.monitoring.svc.cluster.local' \
  --set 'datasources.datasources\.yaml.datasources[0].access=proxy' \
  --set 'datasources.datasources\.yaml.datasources[0].isDefault=true'
```

---

## Accessing Dashboards

### Grafana

Port-forward the Grafana service:

```bash
kubectl port-forward -n monitoring service/grafana 3000:80
```

Then open [http://localhost:3000](http://localhost:3000) in your browser.

**Credentials**:
- **Username**: `admin`
- **Password**: `admin`
  ```

---

## To Uninstall Everything

```bash
helm uninstall grafana -n monitoring
k3d cluster delete
```

---
