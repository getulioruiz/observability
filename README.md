# Próximos Passos para Estudo

1. Instalar o [Prometheus Operator](https://github.com/prometheus-operator/prometheus-operator).
2. Instalar o Prometheus utilizando o Prometheus Operator.
   2.1. Configurar os campos `serviceMonitorSelector` e `podMonitorSelector` conforme necessário.
3. Instalar individualmente o [Node Exporter](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-node-exporter).
4. Instalar individualmente o [Kube State Metrics](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics).
5. Criar um `ServiceMonitor` para o Node Exporter.
6. Instalar o Grafana e configurar alertas no Grafana.
7. Adicionar o Prometheus como datasource no Grafana e visualizar as métricas do Node Exporter.
8. Instalar o Grafana Operator.
9. Criar um alerta e um dashboard utilizando o Grafana Operator.
