# 🚀 EKS Observability Platform  
![Logo](https://img.icons8.com/color/96/aws.png)

# 🌐 Production-Grade EKS Cluster & Observability Stack  
Prometheus • Grafana • Alertmanager • CloudWatch Exporter • EFK Logging • Fluent Bit  

---

## 🔰 Badges

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?logo=amazon-aws)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326ce5?logo=kubernetes)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-orange?logo=prometheus)
![Grafana](https://img.shields.io/badge/Grafana-Dashboards-orange?logo=grafana)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-Logging-005571?logo=elasticsearch)
![FluentBit](https://img.shields.io/badge/Fluent--Bit-LogForwarder-blue)
![Terraform](https://img.shields.io/badge/Terraform-IaC-5C4EE5?logo=terraform)
![GitHubActions](https://img.shields.io/badge/GitHub_Actions-CI%2FCD-blue?logo=githubactions)
![Linux](https://img.shields.io/badge/Linux-Admin-yellow?logo=linux)

---

## 📘 Overview

This repository provides a **production-ready full-stack observability platform** for Amazon EKS, built with:

- Prometheus Operator (metrics)
- Grafana dashboards (visualization)
- CloudWatch exporter (AWS metrics)
- Fluent Bit (log shipping)
- Elasticsearch + Kibana (log analytics)
- Alertmanager (notifications)
- SLO / KPI dashboards

---

# 🧱 Architecture Diagram

```
                              ┌───────────────────────────────┐
                              │          Amazon EKS            │
                              │  (Worker Nodes + Applications) │
                              └───────────────┬────────────────┘
                                              │
                              ┌───────────────▼────────────────┐
                              │     Prometheus Operator         │
                              │  (Metrics, Rules, Alerts)       │
                              └──────┬──────────────┬──────────┘
                                     │              │
                                     │              │
                            ┌────────┘        ┌─────▼──────────┐
                            │                 │     Grafana     │
                            │                 │  Dashboards     │
                            │                 └──────────────────┘
                            │
           ┌────────────────▼────────────────┐
           │           Alertmanager          │
           │ (Slack / Email / Webhook Alerts)│
           └─────────────────────────────────┘


 ┌─────────────────────────────────────────────────────────────┐
 │                           Logging                           │
 ├─────────────────────────────────────────────────────────────┤
 │  Fluent Bit → Elasticsearch → Kibana                        │
 │  (Pod logs)        (Indexing)         (Visualization)       │
 └─────────────────────────────────────────────────────────────┘

 AWS CloudWatch → CloudWatch Exporter → Prometheus
```

---

# 🔁 Flow Chart

```
User → EKS Cluster → Workload Metrics (/metrics) → Prometheus
       │
       └→ Pod Logs → Fluent Bit → Elasticsearch → Kibana UI

Prometheus Rules → Alertmanager → Slack / Email

Grafana ←────── Prometheus Metrics + CloudWatch Metrics
```



---

# 🚀 Quick Start

### 1️⃣ Create EKS Cluster
```bash
eksctl create cluster -f eks/cluster-config.yaml
```

### 2️⃣ Install Prometheus + Grafana
```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring -f monitoring/values-prometheus.yaml
```

### 3️⃣ Install Elasticsearch + Kibana
```bash
kubectl apply -f logging/es-tiny.yaml
kubectl apply -f logging/kibana-minimal.yaml
```

### 4️⃣ Install Fluent Bit
```bash
helm install fluent-bit fluent/fluent-bit -n logging -f logging/fluent-bit-values.yaml
```

---

# 📊 PromQL Queries (SLO / KPI)

### 🔹 P95 Latency
```promql
histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))
```

### 🔹 Error Rate %
```promql
(
  sum(rate(prometheus_sd_kubernetes_http_request_total{status=~"5.."}[5m])) /
  sum(rate(prometheus_sd_kubernetes_http_request_total[5m]))
) * 100
```

### 🔹 Pod Restarts
```promql
sum(increase(kube_pod_container_status_restarts_total[10m])) by (namespace, pod)
```

---

# 🛠 Troubleshooting

| Issue | Reason | Fix |
|------|--------|-----|
| Pods Pending | Memory shortage | Reduce resources / disable node-exporter |
| Webhook errors | Admission job cannot schedule | Patch kube-prometheus-stack |
| ES stuck Pending | High memory requirements | Use es-tiny.yaml |
| Fluent Bit spawns many pods | DS on all nodes | Add nodeSelector |

---

# 🎯 Features

✔ Runs on Free Tier nodes  
✔ Production-ready Prometheus Operator  
✔ Full EFK logging  
✔ SLO dashboards  
✔ Alerting automation  
✔ AWS + Kubernetes unified monitoring  

---

# 🧩 License
MIT License

---

# 👨‍💻 Author  
**Hawlader Md Quamruzzaman**  
Cloud • DevOps • SRE | AWS | Kubernetes | Observability
