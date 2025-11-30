                     ┌────────────────────────────────────┐
                     │            BASTION HOST             │
                     │   • Grafana (UI)                    │
                     │   • Loki                            │
                     └──────────────┬──────────────────────┘
                                    │
                                    │ Push Logs
                                    ▼
                         ┌────────────────────┐
                         │     PROMTAIL       │
                         │ (DaemonSet on EKS) │
                         └────────────────────┘
                                    │
                                    │ Scrape Logs
                                    │
    ┌─────────────────────────────────────────────────────────────────────┐
    │                     PRIVATE EKS CLUSTER (No Public Access)          │
    │                                                                     │
    │   ┌────────────────────┐      ┌──────────────────────────┐          │
    │   │ NODE EXPORTER      │      │ KUBE STATE METRICS       │          │
    │   │ (DaemonSet)        │      │ (Deployment)              │          │
    │   └───────────┬────────┘      └─────────────┬────────────┘          │
    │               │ Scrape Metrics              │ Scrape Metrics        │
    │               ▼                              ▼                      │
    │        ┌───────────────────┐         ┌───────────────────┐          │
    │        │   CADVISOR        │         │ SERVICE / POD      │          │
    │        │  (PodMonitor)     │         │ MONITORS           │          │
    │        └─────────┬─────────┘         └─────────┬─────────┘          │
    │                  │ Auto-Discovery via Prometheus Operator           │
    │                  ▼                                                  │
    │        ┌────────────────────────────┐                                │
    │        │  PROMETHEUS OPERATOR       │                                │
    │        │  (from kube-prom-stack)    │                                │
    │        └──────────────┬─────────────┘                                │
    │                       │ Creates Prometheus StatefulSet               │
    │                       ▼                                              │
    │        ┌────────────────────────────────┐                            │
    │        │        PROMETHEUS (StatefulSet)│                            │
    │        │   • Uses gp3 StorageClass      │                            │
    │        │   • PVC via EBS CSI driver     │                            │
    │        │   • IRSA → Role → EBS Access   │                            │
    │        └──────────────┬─────────────────┘                            │
    │                       │                                               │
    │                       ▼                                               │
    │             ┌────────────────────────┐                                 │
    │             │ NGINX INGRESS + TLS    │                                 │
    │             │ cert-manager + Issuer  │                                 │
    │             └────────────────────────┘                                 │
    └─────────────────────────────────────────────────────────────────────────┘
