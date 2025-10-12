
# 🚀 Kubernetes Detailed Interview Q&A (For 2 Years DevOps Experience)

This document covers detailed explanations of **Kubernetes interview questions** with real project-based examples, written in an easy-to-speak format for practical interviews.

---

## 🧩 1. ConfigMap vs Secret

**ConfigMap** stores non-sensitive configuration data like environment variables and application settings, while **Secrets** store sensitive data like passwords or API keys in base64 format.  
In my setup, I used Secrets for Prometheus and Grafana credentials, and ConfigMaps for YAML configurations. For higher security, sensitive data was managed in AWS Secret Manager and accessed via IRSA.

---

## 🧩 2. Persistent Volume (PV) vs Persistent Volume Claim (PVC) + gp3 EBS Integration

**Persistent Volumes (PV)** represent actual storage in the cluster, while **Persistent Volume Claims (PVC)** are requests made by pods to claim that storage.  
In my EKS monitoring setup, I configured the **EBS CSI driver** using **gp3** storage type with IRSA-enabled service accounts, ensuring that Prometheus and Loki had secure, dedicated, and persistent storage.

---

## 🧩 3. RBAC (Role-Based Access Control)

**RBAC** restricts access to Kubernetes resources based on user roles and permissions.  
I implemented RBAC by creating custom Roles and RoleBindings for Service Accounts used by Prometheus Operator, allowing it read-only access to Kubernetes APIs for metrics collection without granting cluster-wide admin rights.

---

## 🧩 4. Prometheus, Grafana, Loki, and Promtail Integration

I implemented a hybrid monitoring setup using **Prometheus, Grafana, Loki, and Promtail**:  
Prometheus collected metrics from Node Exporter, Kube-State-Metrics, and cAdvisor. Promtail ran as a DaemonSet on all nodes to collect pod logs and push them to Loki, which Grafana visualized via dashboards. TLS certificates for Grafana were managed using cert-manager with Let’s Encrypt.

---

## 🧩 5. Pod Troubleshooting (CrashLoopBackOff, Image Issues)

When a pod is stuck in **CrashLoopBackOff**, I check its description and logs using:
```bash
kubectl describe pod <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> --tail=50
```
Common causes include incorrect image, probe misconfiguration, or insufficient resources. I fix these by updating the deployment manifest, verifying image registry access, and reapplying the corrected configuration.

---

## 🧩 6. Service Account & IRSA in EKS

**Service Accounts** provide pod-level identity and permissions for accessing Kubernetes APIs.  
In AWS EKS, **IRSA (IAM Role for Service Account)** maps an IAM role to a Kubernetes Service Account, allowing pods to securely access AWS services like S3 or EBS without storing credentials in Secrets.

---

## 🧩 7. CNI Plugin

The **Container Network Interface (CPI)** plugin handles networking between pods and nodes in a cluster.  
It assigns IPs to pods and controls traffic routing. Common CNIs include **Amazon VPC CNI**, **Calico**, and **Flannel**. In my project, Amazon VPC CNI was used for native networking integration in EKS.

---

## 🧩 8. Node Affinity, Taints & Tolerations

**Taints** prevent pods from being scheduled on certain nodes, while **Tolerations** allow specific pods to tolerate those taints.  
**Node Affinity** ensures pods are scheduled on nodes with specific labels. These features are used for workload isolation, ensuring high-priority pods run only on dedicated nodes.

---

## 🧩 9. Service Types (ClusterIP, NodePort, LoadBalancer)

- **ClusterIP:** Exposes service internally within the cluster.  
- **NodePort:** Opens a specific port on all nodes to expose service externally.  
- **LoadBalancer:** Integrates with cloud provider load balancers (e.g., AWS ELB, GCP LB) for external traffic routing.  
In my setup, I used LoadBalancer + Ingress for HTTPS access via TLS.

---

## 🧩 10. Autoscaling (HPA, VPA, Cluster Autoscaler)

- **HPA (Horizontal Pod Autoscaler):** Scales pods based on CPU/memory usage.  
- **VPA (Vertical Pod Autoscaler):** Adjusts pod CPU/memory requests dynamically.  
- **Cluster Autoscaler:** Adds or removes nodes based on pending pods.  
In EKS, I used HPA for Prometheus replicas and Cluster Autoscaler for node scaling under load.

---

## 🧩 11. Ingress Controller vs LoadBalancer

An **Ingress Controller** routes external HTTP/HTTPS traffic to internal services using rules and hostnames, while **LoadBalancer** exposes a single service publicly.  
I used **NGINX Ingress Controller** with **cert-manager** for TLS and domain routing in my project.

---

## 🧩 12. Resource Usage Monitoring

To view CPU/memory usage for pods and nodes:
```bash
kubectl top pods -A
kubectl top nodes
```
Requires **metrics-server** installed. I integrated Prometheus metrics with Grafana dashboards for long-term performance tracking.

---

## 🧩 13. Troubleshooting Summary

If pod or service issues occur:
1. Check status: `kubectl get pods -A`  
2. Describe details: `kubectl describe pod <pod>`  
3. Check logs: `kubectl logs <pod>`  
4. Verify events and probes.  
Most issues are due to misconfigured images, missing secrets, or resource constraints.

---

## 🏁 Summary Table

| Concept | Summary |
|----------|----------|
| ConfigMap vs Secret | ConfigMap = config data, Secret = sensitive data |
| PV vs PVC | PV = storage, PVC = request |
| IRSA | Secure AWS access for pods |
| RBAC | Restricts access to K8s APIs |
| Prometheus Stack | Metrics + Logs + Dashboards |
| CrashLoopBackOff | Repeated pod crash; check logs & probes |
| Node Affinity/Taints | Pod placement control |
| CNI | Handles pod networking |
| Service Types | ClusterIP, NodePort, LoadBalancer |
| Autoscaling | HPA, VPA, Cluster Autoscaler |

---

**Prepared by:** Saravana L  
*(Kubernetes + Monitoring + EKS Hands-on Experience)*
