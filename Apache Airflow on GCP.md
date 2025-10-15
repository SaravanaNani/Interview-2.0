# Interview_Questions_and_Answers.md

# 🎯 Apache Airflow on GCP – Terraform Infrastructure Interview Q&A

This document provides **real-world interview questions and answers** based on your **Apache Airflow production setup on GCP** using Terraform, Secret Manager, Cloud SQL, GCS, and Compute Engine automation.

---

## 🧱 Infrastructure Design & Terraform

### 1️⃣ **Q:** How did you architect Apache Airflow on GCP?

**A:**
I built a modular Terraform setup that provisions the entire Airflow infrastructure. It includes:

* VPC, Subnet, and Firewall rules
* Service Account with granular IAM roles
* Cloud SQL (PostgreSQL) for Airflow metadata
* GCS bucket for DAGs and logs
* Compute Engine VM hosting Airflow using Docker Compose
* Secret Manager for database and service credentials
  All components are automated and parameterized for reusability.

---

### 2️⃣ **Q:** Why did you switch from remote-exec provisioners to startup scripts?

**A:**
Terraform’s remote-exec failed due to SSH authentication timing issues. I replaced it with a **metadata startup script**, which executes automatically during VM initialization. This made the deployment more reliable, network-independent, and scalable.

---

### 3️⃣ **Q:** What Terraform modules did you design?

**A:**

1. **VPC Module** – Creates network and subnet.
2. **Firewall Module** – Opens ports (22, 80, 8080).
3. **Service Account Module** – Generates IAM account and roles.
4. **SQL Instance Module** – Deploys Cloud SQL (PostgreSQL 14).
5. **GCS Bucket Module** – Stores Airflow DAGs/logs.
6. **Secret Manager Module** – Manages credentials.
7. **Airflow VM Module** – Creates the Compute Engine instance and passes metadata to the startup script.

---

### 4️⃣ **Q:** How did you manage sensitive data like DB credentials?

**A:**
I used **Google Secret Manager** for storing credentials. The VM fetches credentials dynamically at boot time using the metadata server and access token:

```bash
ACCESS_TOKEN=$(curl -s -H "Metadata-Flavor: Google" http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token | jq -r .access_token)
DB_PASSWORD=$(curl -s -H "Authorization: Bearer $ACCESS_TOKEN" "https://secretmanager.googleapis.com/v1/projects/${PROJECT_ID}/secrets/db-password/versions/latest:access" | jq -r .payload.data | base64 --decode)
```

This eliminated hardcoding credentials in Terraform or Docker Compose.

---

### 5️⃣ **Q:** Which IAM roles were essential for the Airflow service account?

**A:**

```hcl
roles = [
  "roles/compute.instanceAdmin.v1",
  "roles/storage.admin",
  "roles/cloudsql.client",
  "roles/secretmanager.secretAccessor",
  "roles/logging.logWriter",
  "roles/monitoring.metricWriter"
]
```

These roles enable Airflow to access SQL, read DAGs from GCS, and fetch secrets.

---

## ☁️ GCP Services & Connectivity

### 6️⃣ **Q:** What is the role of Cloud SQL Proxy here?

**A:**
The Cloud SQL Proxy container allows the Airflow VM to connect securely to the Cloud SQL instance using IAM authentication, without exposing public IP credentials. It runs as a sidecar service inside the VM via Docker Compose.

---

### 7️⃣ **Q:** What caused your `connection refused` issue during Airflow startup?

**A:**
The Cloud SQL Proxy was not ready when Airflow tried to connect. To fix it, I added a **network availability check** and ensured Airflow containers start only after the proxy is healthy:

```bash
until nc -z 127.0.0.1 5432; do
  echo "Waiting for Cloud SQL Proxy..."; sleep 5; done
```

---

### 8️⃣ **Q:** How did you validate the startup script’s execution?

**A:**
I verified using:

```bash
cat /var/log/startup-script.log
```

This log file contains every step output (package installs, gcloud setup, Docker execution) to confirm if the script ran successfully.

---

## 🧩 Airflow Deployment & Configuration

### 9️⃣ **Q:** How did you deploy Airflow in the VM?

**A:**
I used **Docker Compose** with 4 services:

1. **cloudsql-proxy** – SQL connection bridge
2. **airflow-init** – Initializes metadata DB
3. **airflow-webserver** – UI service (port 8080)
4. **airflow-scheduler** – Task scheduling
   The startup script automatically brings up all services.

---

### 🔟 **Q:** How did you sync DAGs between GCS and Airflow?

**A:**
I used `gsutil rsync` to sync DAGs from the GCS bucket to `/opt/airflow/dags` on the VM:

```bash
gsutil -m rsync -r gs://${GCS_BUCKET}/dags/ /opt/airflow/dags
```

This ensures the VM always runs the latest DAGs.

---

## 🧰 Troubleshooting & Optimization

### 11️⃣ **Q:** How did you debug Docker container issues?

**A:**
Using Docker logs:

```bash
docker ps -a
docker logs airflow-cloudsql-proxy-1
docker logs airflow-airflow-init-1
```

Also validated Cloud SQL logs from GCP Console.

---

### 12️⃣ **Q:** What was the most critical issue you faced?

**A:**
Airflow couldn’t reach Cloud SQL because the proxy command used `-nstances` instead of `-instances`. Fixing the command and ensuring proper credentials resolved it.

---

### 13️⃣ **Q:** What are the common startup script pitfalls?

**A:**

* Script not marked executable
* Missing `#!/bin/bash` or `set -e`
* Commands running before network availability
* Not logging output (`exec > /var/log/startup-script.log 2>&1`)

---

## 🔒 Security & Best Practices

### 14️⃣ **Q:** How did you ensure the environment is secure?

**A:**

* Used private VPC for isolation.
* No SSH access via public IP unless explicitly needed.
* Credentials stored only in Secret Manager.
* Service accounts have minimal IAM roles.

---

### 15️⃣ **Q:** Why is this setup considered production-grade?

**A:**

* Modular, reusable Terraform code.
* Secure credential management.
* Automatic dependency checks.
* Decoupled provisioning (Infra + Application).
* Easily scalable by adjusting Terraform variables.

---

## 🧾 Bonus Questions

### 16️⃣ **Q:** How can this setup be extended for High Availability?

**A:**

* Use **GKE (Kubernetes)** instead of a single VM.
* Store Airflow logs and DAGs in **GCS** (already integrated).
* Deploy **multiple schedulers** and **workers** via Docker Compose scaling.

---

### 17️⃣ **Q:** What’s the benefit of using Secret Manager over Terraform variables?

**A:**
Secrets stay encrypted and isolated from Terraform state files. They can be rotated independently, and only authorized service accounts can access them at runtime.

---

### 18️⃣ **Q:** How do you perform maintenance or redeploys?

**A:**

* Use `terraform apply` for infra changes.
* Update DAGs in GCS directly.
* Restart VM for startup script re-execution if needed.

---

### 19️⃣ **Q:** Which logs are most useful during debugging?

**A:**

* `/var/log/startup-script.log`
* `docker logs airflow-*`
* GCP Logs: Cloud SQL, Compute Engine, Secret Manager access logs.

---

### 20️⃣ **Q:** What’s your biggest learning from this project?

**A:**
Avoid tight coupling between Terraform and runtime scripts. Startup scripts, metadata, and Secret Manager integration make infrastructure resilient and fully automated.

---

**📘 Summary:**
This setup demonstrates complete automation of an Airflow deployment on GCP using best DevOps practices — from Terraform modularity, secure secrets handling, and network configuration to troubleshooting production-level issues.
