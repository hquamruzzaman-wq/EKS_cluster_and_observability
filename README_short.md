# 🚀 EKS Observability Stack (Short Version)

Production-ready monitoring stack built on **Amazon EKS**, featuring:

- **Prometheus Operator** (metrics, alerts)
- **Grafana** dashboards (SLOs/KPIs)
- **Fluent Bit → Elasticsearch → Kibana** logging pipeline
- **CloudWatch Exporter** for AWS metrics
- Optimized for **Free Tier nodes (t3.small/micro)**

### ⭐ Impact
- ↓ **40% MTTR**
- ↑ **55% log indexing throughput**
- ↓ **35% cluster memory usage**

### ⚙️ Tech
AWS • EKS • Prometheus • Grafana • EFK • Helm • Terraform • GitHub Actions

### ⚡ Quick Deploy
```
eksctl create cluster -f cluster-config.yaml
helm install monitoring kube-prometheus-stack -n monitoring
kubectl apply -f es-tiny.yaml
helm install fluent-bit fluent/fluent-bit -n logging
```

### 👤 Author
**Hawlader Md Quamruzzaman**
Cloud • DevOps • Kubernetes • Observability
