# Cert-manger

“cert-manager is a custom Kubernetes controller that manages certificates. When I create a ClusterIssuer, it becomes a cluster-wide certificate authority configuration. Any Ingress can request a certificate simply by adding the cert-manager annotation. cert-manager sees this, creates a Certificate resource, matches it with the ClusterIssuer, and then communicates with Let’s Encrypt using ACME challenges. After validation, cert-manager stores the certificate in a Kubernetes Secret, and the Ingress uses that secret to serve HTTPS.”


# Prometheus
EKS Observability & Logging Stack (Internal Setup – Private Cluster)

Tech Stack: Helm, Prometheus Operator, Grafana, Loki, Promtail, Node Exporter, Kube-State-Metrics, cAdvisor, cert-manager, EBS CSI, IRSA, NGINX Ingress

Installed kube-prometheus-stack using Helm and configured Prometheus Operator for custom monitoring requirements.

Deployed Node Exporter, Kube-State-Metrics, and cAdvisor manually to support metric collection in a private EKS cluster.

Created ServiceMonitors and PodMonitors for automatic target discovery and metric scraping.

Configured Prometheus with persistent storage using an EBS-backed StorageClass via the EBS CSI driver, secured with IRSA-based IAM roles.

Set up Promtail DaemonSet for cluster log collection and integrated it with Loki running on a bastion host.

Exposed Prometheus through NGINX Ingress, enabling HTTPS using cert-manager and Let’s Encrypt ClusterIssuer.

Achieved complete internal observability including node health, pod performance, cluster state, and system logs.
                     
                     
                     ┌────────────────────────────────────┐
                     │            BASTION HOST            │
                     │   • Grafana (UI)                   │
                     │   • Loki                           │
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
    │                     PRIVATE EKS CLUSTER (No Public Access)           │
    │                                                                      │
    │   ┌────────────────────┐      ┌──────────────────────────┐           │
    │   │ NODE EXPORTER      │      │ KUBE STATE METRICS       │           │
    │   │ (DaemonSet)        │      │ (Deployment)             │           │
    │   └───────────┬────────┘      └─────────────┬────────────┘           │
    │               │ Scrape Metrics              │ Scrape Metrics         │
    │               ▼                              ▼                       │
    │        ┌───────────────────┐         ┌───────────────────┐           │
    │        │   CADVISOR        │         │ SERVICE / POD     │           │
    │        │  (PodMonitor)     │         │ MONITORS          │           │
    │        └─────────┬─────────┘         └─────────┬─────────┘           │
    │                  │ Auto-Discovery via Prometheus Operator            │
    │                  ▼                                                   │
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
    │                       │                                              │
    │                       ▼                                              │
    │             ┌────────────────────────┐                               │
    │             │ NGINX INGRESS + TLS    │                               │
    │             │ cert-manager + Issuer  │                               │
    │             └────────────────────────┘                                │
    └─────────────────────────────────────────────────────────────────────────┘

“In our internal EKS environment, I built a complete monitoring and logging stack. I deployed the kube-prometheus-stack using Helm, which installed Prometheus Operator, Grafana, and default exporters. Since we were using a private EKS cluster, I manually deployed Node Exporter, Kube-State-Metrics, and cAdvisor using DaemonSets and Deployments.

I also created custom ServiceMonitors and PodMonitors so that Prometheus Operator could automatically discover and scrape metrics from these components. For Prometheus itself, I created a custom Prometheus CRD with a dedicated storage class, RBAC, and ServiceAccount designed for IRSA integration.

For logging, I deployed Promtail as a DaemonSet and pushed logs to a Loki instance running on the bastion host. Finally, I exposed Prometheus using NGINX Ingress with cert-manager TLS certificates. Overall, I implemented the complete observability layer—metrics, logs, and dashboards—for a private EKS setup.”
