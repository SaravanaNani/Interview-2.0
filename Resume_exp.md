# DevOps Interview Preparation (2 Years Experience) – Saravana L

This document contains 25 core **DevOps interview questions and refined answers** based on real experience, plus additional quick notes on important tools, Terraform, Jenkins plugins, and troubleshooting scenarios.

---

## 🧑‍💼 1. Self Introduction

> “Hi, I’m Saravana, a Cloud and DevOps Engineer with around 2 years of experience. I’ve worked on automating infrastructure on GCP and AWS using Terraform and Ansible, building CI/CD pipelines with Jenkins, and containerizing deployments with Docker and Kubernetes (EKS). I’ve also set up monitoring using Prometheus, Grafana, and Loki, and written Python scripts for small automation tasks.”

---

## ⚙️ 2. Jenkins CI/CD Pipeline Flow

* Pipeline stages: **Environment Setup → Code Quality → Build → Docker → Image Scan → Deploy (EKS)**
* Multi-stage Dockerfile used for optimized builds
* Each stage includes email notifications and approvals

**Example:** Maven builds WAR → Docker image created → pushed to Nexus → scanned → deployed to EKS.

---

## 🐳 3. Why Use Docker Agent in Jenkins

* Lightweight, isolated, and ephemeral
* Runs builds faster using shared kernel
* Avoids VM setup and manual dependency installations

**Example:** Each build runs in a clean Docker agent with preinstalled Terraform/Ansible, ensuring consistency.

---

## 🌍 4. Terraform Modules

* Used to **reuse infrastructure code** for multiple environments (dev, stage, prod)
* Common modules: VPC, IAM roles, storage buckets
* Reduces duplication and enforces standardization

**Example:** One VPC module reused across 3 environments with different CIDR values.

---

## 📦 5. Terraform State Management

* State files track real infrastructure vs code.
* Stored in **remote backend** (S3/GCS) for team access.
* **DynamoDB** used for state locking (prevents multiple applies).

**Example:** S3 keeps versioned states; if a teammate runs Terraform simultaneously, state lock avoids overwrite.

---

## 🤖 6. Ansible Dynamic Inventory (GCP)

* Used **GCP Compute plugin** to auto-fetch VMs by tags.
* Roles and handlers applied configurations dynamically.

**Example:** Automatically fetch all VMs tagged `env=dev` and install dependencies via roles.

---

## 🧩 7. Real Error Faced in Jenkins

* Build succeeded but deployment failed due to **Tomcat not reachable**.
* Added a pre-deploy script to check Tomcat readiness.
* Deployment succeeded afterward.

---

## 🐳 8. Multi-Stage Docker Build

* Used multiple `FROM` stages to reduce image size.
* First stage builds WAR → second stage runs lightweight Tomcat.

**Benefit:** Smaller images, faster deployments, fewer dependency issues.

---

## ☸️ 9. EKS Deployment Flow

1. Build Docker image → Push to Registry
2. Jenkins deploys via `kubectl` or Helm
3. Service + Ingress expose app via ALB
4. Logs via Promtail → Loki, Metrics via Prometheus → Grafana

---

## 🔁 10. HPA vs Cluster Autoscaler

* **HPA:** Scales pods based on CPU/memory.
* **CA:** Adds/removes nodes if pods can’t be scheduled.

**Example:** HPA scales from 3→6 pods, CA adds a new EC2 node when resources are insufficient.

---

## 📊 11. Prometheus + Grafana + Loki Stack

* **Prometheus:** Collects metrics (Node Exporter, cAdvisor)
* **Promtail:** Collects logs → **Loki** stores them
* **Grafana:** Visualizes metrics and logs; sets alerts.

**Flow:** Promtail → Loki → Grafana (logs), Prometheus → Grafana (metrics)

---

## 🔍 12. SonarQube Integration in Jenkins

* Jenkins triggers SonarQube scan via token.
* Checks code quality (bugs, smells, vulnerabilities).
* Fails build if quality gate not passed and emails dev team.

---

## ⚡ 13. Ansible Handlers and Notify

* `notify` triggers `handlers` only when a change occurs.

**Example:** Updating Tomcat config → triggers handler to restart Tomcat only if config changed.

---

## 🧱 14. Container Exits Immediately (Debug)

1. Check logs: `docker logs <id>`
2. Inspect container: `docker inspect <id>`
3. Verify CMD/ENTRYPOINT (use `run` instead of `start`)
4. Rebuild image if needed

**Example:** Tomcat container exited because `catalina.sh start` used instead of `run`.

---

## ☸️ 15. EKS App Not Accessible

1. Check pods: `kubectl get pods`
2. Check logs/describes
3. Validate Service & Ingress rules
4. Check ALB or NLB health
5. Rollback deployment if faulty image.

**Example:** Wrong target port in Service caused ingress health check failure.

---

## ⚙️ 16. Random Jenkins Failures

* Usually due to cached workspaces.
* Use `cleanWs()` before build.
* Ensures every build starts fresh.

---

## 📦 17. Nexus Artifact Upload

* After build, artifact uploaded via Nexus plugin or Maven deploy.
* Versioning via `JAVA_APP-1.2.${BUILD_NUMBER}.war`
* Nexus acts as central artifact storage.

---

## ☁️ 18. Terraform Delete Recovery

* Restore from **state backup** (S3/GCS versioning)
* Prevent by using `prevent_destroy` and reviewing `plan`.
* Use DynamoDB state locks to avoid parallel apply.

---

## 🌿 19. Git Branch Strategy

* Feature → PR → Develop → Main
* Webhooks trigger Jenkins builds for PRs.
* Email alerts for success/failure.

---

## 🔐 20. Secrets Management

* Use Jenkins credentials or Vault/Secrets Manager.
* Inject credentials dynamically into pipelines.
* Avoid hardcoding sensitive data.

---

## 🧠 21. Real Challenge Faced (Promtail)

* Promtail couldn’t collect logs.
* Found EKS using **containerd**, not Docker.
* Updated log paths in ConfigMap → issue resolved.
* Learned: Runtime differences matter in observability setup.

---

## 💻 22. Infrastructure as Code (IaC)

* Automates infra creation, saves time.
* Ensures consistency and version control.
* Easy to modify and reuse.

**Example:** Same Terraform module for Dev/Prod using different variable files.

---

## 🙋‍♂️ 23. Why Should We Consider You?

> “I have hands-on experience with core DevOps tools and multi-cloud automation. I adapt quickly to new tech — for example, I learned EKS and GCP within my role. I bring both technical skill and a strong problem-solving mindset.”

---

## ⚡ 24. Optimizing Slow Jenkins Builds

* Run independent stages **in parallel**
* Clean workspace (`cleanWs()`)
* Use **cached dependencies**
* Use **lightweight Docker agents**
* Trigger builds only when required

**Example:** Parallelizing Maven + Trivy scans cut build time by 40%.

---

## 📚 25. Staying Updated in DevOps

> “I stay updated by troubleshooting real issues, exploring new tools, and testing alternate solutions. Each problem I solve teaches me something new about DevOps and automation.”

---

# 🧩 Quick Reference Notes

### 🔌 Must-Know Jenkins Plugins

* **Pipeline Plugin** – Declarative & scripted pipelines
* **Git Plugin** – SCM integration
* **Nexus Artifact Uploader** – Publish artifacts
* **SonarQube Plugin** – Code analysis
* **Email-ext Plugin** – Notifications
* **Credentials Binding Plugin** – Secret injection

---

### 🌍 Terraform State File – Quick Summary

* Tracks infrastructure created by Terraform.
* Helps Terraform know what already exists to avoid recreation.
* Stored in remote backend for collaboration.

**Example:** When `terraform apply` runs, Terraform compares state with config → updates only what changed.

**State.lock (with DynamoDB)**:

* Prevents multiple users from applying changes at the same time.
* Example: If user A runs apply, user B’s apply waits until lock released.

---

### 💡 Real-Time Example of State Management

**Scenario:** Two engineers applying changes to the same VPC.

* Without lock → race condition, resource conflicts.
* With DynamoDB lock → second apply waits; no overlap.

---

### 🧠 Common Troubleshooting Topics

* **CrashLoopBackOff:** Check ConfigMap, image tag, liveness probe.
* **ImagePullBackOff:** Registry credentials or wrong tag.
* **Terraform Error:** Validate variables & state sync.
* **Jenkins Failures:** Clean workspace, check agent logs.

---

### 🌐 Core Cloud Commands (Quick Recall)

| Tool                                     | Command               | Purpose |
| ---------------------------------------- | --------------------- | ------- |
| `kubectl get pods`                       | Check pod status      |         |
| `kubectl describe pod <name>`            | View pod events       |         |
| `terraform plan`                         | Preview infra changes |         |
| `ansible-playbook site.yml -i inventory` | Run playbook          |         |
| `docker logs <id>`                       | View container logs   |         |

---

### ✅ Interview Tip

Keep your answers short, real, and experience-based. Add one real-world example per topic — that’s what differentiates a **hands-on DevOps engineer** from a theoretical one.

---

**Prepared by:** Saravana L
**Role:** Cloud & DevOps Engineer
**Experience:** 2 Years (AWS, GCP, Terraform, Jenkins, Docker, EKS, Ansible)
