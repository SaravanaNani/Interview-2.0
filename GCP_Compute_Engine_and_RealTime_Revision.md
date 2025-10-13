
# ☁️ GCP Compute Engine & Real-Time Implementation – Interview Revision Guide

---

## ⚙️ GCP Compute Engine Recap

### 💡 General Tips
- 1 Core = 2 vCPUs in GCP
- **Sustained Use Discount (SUD):** Up to 30% off for long-running VMs
- **Committed Use Discount:** Up to 57% off for 1–3 year commitment
- **Startup scripts** always run as **root (Linux)** or **Administrator (Windows)**
- **Flag for Preemptible VM:** `--preemptible`
- **Disable boot disk auto-delete:** `--no-boot-disk-auto-delete`
- **Add data disk:** `--create-disk=mode=rw,auto-delete=yes`

---

### 🖥️ Compute Models
| Layer | Description |
|--------|--------------|
| IaaS | Compute Engine (manage VMs) |
| CaaS | Kubernetes Engine (manage containers) |
| PaaS | App Engine (manage apps) |
| FaaS | Cloud Functions (manage functions) |

---

### 💾 Disk Overview
- **Network-attached:** PD Standard, SSD, Balanced PD — resizable, detachable
- **Local SSD:** High IOPS, but data lost on restart
- **Encryption:** Google-managed (default), Customer-managed (KMS), or Customer-supplied (on-prem)
- **Boot Disk:** OS only; **Data Disk:** for persistent storage

---

### 🧱 VM Classes
| Type | Description |
|------|--------------|
| Regular | Standard VMs |
| Preemptible | Short-lived (max 24h), 80% cheaper, ideal for fault-tolerant workloads |

**Graceful shutdown:** 30s to save data → use startup/shutdown scripts for backups.

---

### 🧠 Exam Tips
- Start VMs early in month → maximize SUD
- Local SSD only at creation time
- Always use **shutdown script** to backup preemptible VM data
- Use `gcloud compute instances describe` to inspect VM metadata

---

## 🔧 Real-Time Implementation Scenarios

### 🔹 Expand Disk Without Downtime
```bash
sudo apt -y install cloud-guest-utils
sudo growpart /dev/sda1
sudo resize2fs /dev/sda1
```
✅ Increase size in console first.  
⚠️ Disks cannot be reduced — start smaller and grow.

---

### 🔹 Provision Preemptible VM
```bash
gcloud compute instances create preempt-vm --preemptible --machine-type=f1-micro --zone=us-central1-a
```
✅ Cheaper & short-lived; restarting restarts 24h timer.

---

### 🔹 Move VM Between Zones (Same Region)
```bash
gcloud compute instances move VM_NAME --zone=us-central1-a --destination-zone=us-central1-b
```
⚠️ Works only for *unshielded VMs*.  
🛠️ Shielded VMs → Use **snapshot + static IP** method.

---

### 🔹 Cross-Region Migration (Zero Downtime)
- Create snapshot → Launch new VM in new region → Update DNS with new IP.

---

### 🔹 Migrate VM Across Projects
- Snapshots = project-local  
- Use **Custom Image** for cross-project sharing (needs `compute.imageUser` role).

---

### 🧩 Snapshot vs Custom Image
| Feature | Snapshot | Custom Image |
|----------|-----------|--------------|
| Scope | Same project | Multi-project |
| Type | Incremental | Full copy |
| Cost | Cheaper | Expensive |
| Use Case | Backup & recovery | VM reuse |

---

### 💾 Machine Image
- Stores config, metadata, and disks for full VM recovery or cloning.

---

### 🧱 Instance Groups
- **MIG:** Auto-healing, load-balanced, auto-scaling.  
- **UMIG:** Manual scaling, used for migrations.  
- **Regional MIG:** Multi-zone HA setup.

---

### ⚙️ Real-Time GCP Operations
- Use **dos2unix** to fix Windows file format issues.  
- **Freeze VM** before taking snapshot.  
- Use **default region/zone** for faster demo setup.  
- **Local SSDs** cannot be attached post-creation.  

---

### 🧠 Containerization & Registry

#### Why Containers?
- Lightweight, portable, cloud-agnostic.
- Faster rollbacks and better environment parity.

#### Docker Basics
```bash
docker build -t gcr.io/$PROJECT_ID/app:v1 .
docker push gcr.io/$PROJECT_ID/app:v1
docker pull gcr.io/$PROJECT_ID/app:v1
docker exec -it container bash
```
✅ Use **Alpine** for lightweight base images.  
⚠️ Avoid `latest` tag; use semantic versioning (`v1.2.0`).

---

### 🧱 Artifact & Container Registry
- **Artifact Registry:** Stores Docker + Maven/NPM artifacts.  
- **Container Registry (GCR):** Legacy version; creates GCS bucket behind the scenes.

---

### 🧩 Cloud Build
```bash
gcloud builds submit -t gcr.io/$PROJECT_ID/myapp:v1
```
✅ Serverless CI/CD; 2 free build hours/day.

---

### ☸️ GKE Overview
- Managed Kubernetes platform → no master node management.  
- Scalable, secure, multi-cloud ready.  
- Supports **declarative (YAML)** and **imperative (kubectl)** configurations.

---

### 🧩 GKE Components
| Component | Description |
|------------|--------------|
| Control Plane | API Server, Scheduler, Controller Manager, etcd |
| Node | Kubelet, kube-proxy, Pods |

---

### 🚀 GKE Deployment Flow
1️⃣ Build Docker image  
2️⃣ Push to GCR  
3️⃣ Create GKE cluster  
4️⃣ Deploy workload  
5️⃣ Expose as Service  
6️⃣ Scale & update versions

---

### ✅ Best Practices
- Never use `latest` in production.  
- Clean unused images from GCR.  
- Use **Private GKE clusters** in production.  
- Enable **auto-scaling, auto-upgrade, and monitoring**.  
- Always set **CPU/memory limits** in Pods.

---

**Prepared by:** Saravana L  
*(DevOps Engineer | GCP | Terraform | Jenkins | Docker | Kubernetes)*
