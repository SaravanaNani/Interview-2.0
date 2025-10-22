# DevOps & Cloud Engineer Interview Q&A (2-3 Years Experience)

## 🧩 Section 1: General DevOps Concepts

**1. What is DevOps and why is it important?**
DevOps is a culture and set of practices that unify software development and operations to shorten the SDLC, improve collaboration, and enable continuous delivery.

**2. What are the main benefits of implementing CI/CD pipelines?**
Faster delivery, improved quality, early bug detection, automation of repetitive tasks, and consistent deployments.

**3. What is the difference between Continuous Integration and Continuous Deployment?**
CI focuses on automatically building and testing code, while CD automates deployment to production after successful tests.

**4. How do you ensure zero downtime deployments?**
By using rolling updates, blue-green deployment strategies, and load balancers to manage traffic during deployment.

---

## ⚙️ Section 2: Infrastructure as Code (Terraform & Ansible)

**1. What is Infrastructure as Code (IaC)?**
IaC is the process of managing and provisioning infrastructure through code rather than manual processes.

**2. How do Terraform and Ansible differ?**
Terraform is declarative and focuses on provisioning infrastructure, while Ansible is procedural and handles configuration management.

**3. Explain Terraform state files and their purpose.**
The state file tracks the current infrastructure deployed by Terraform, allowing it to know what needs to be changed or updated.

**4. What are Terraform modules and how have you used them?**
Modules are reusable components of Terraform code. I used them to deploy VPCs, subnets, and IAM roles across multiple environments.

**5. How do you use Ansible for application deployment?**
By writing playbooks that define configuration tasks, software installation, and service management across multiple VMs.

---

## ☁️ Section 3: Cloud Platforms (AWS & GCP)

**1. Which GCP services did you use for Airflow deployment?**
Used GCE for compute, Cloud SQL as the backend, and GCS for DAG storage.

**2. Explain how you managed IAM and networking on GCP using Terraform.**
Defined IAM roles and VPC networks through Terraform scripts to maintain versioned and consistent configurations.

**3. Describe how you deployed and monitored an application on AWS EKS.**
Deployed Dockerized applications on EKS, used Ingress for routing, and monitored using Prometheus and Grafana.

**4. What’s the difference between AWS EC2 and GCP Compute Engine?**
Both provide virtual machines, but pricing, integration services, and management tools differ slightly.

---

## 🚀 Section 4: CI/CD (Jenkins, Nexus, SonarQube)

**1. How did you automate builds and deployments in Jenkins?**
Created pipelines using Jenkinsfiles to automate build, test, and deploy stages.

**2. What is the role of Nexus in a CI/CD pipeline?**
Acts as an artifact repository for storing build outputs like WAR or JAR files.

**3. How did you integrate SonarQube for code quality checks?**
Used Jenkins plugins and project keys to perform static code analysis during builds.

**4. How do you handle rollbacks in Jenkins pipelines?**
By maintaining versioned artifacts in Nexus and using conditional logic in Jenkins to redeploy stable builds.

---

## 🐳 Section 5: Docker & Kubernetes

**1. How do you create a Docker image and push it to a registry?**
Using a Dockerfile to build the image and `docker push` to upload to DockerHub or ECR.

**2. How do you deploy applications on Kubernetes (EKS)?**
By creating YAML manifests for Deployments, Services, and Ingress resources.

**3. What is the use of ConfigMaps and Secrets in Kubernetes?**
ConfigMaps store non-sensitive configurations, while Secrets store confidential data like passwords.

**4. How do you implement monitoring in Kubernetes using Prometheus and Grafana?**
Deployed Prometheus for metrics collection and Grafana for visualization via Helm charts.

---

## 🧰 Section 6: Monitoring & Logging

**1. Explain how Prometheus, Grafana, and Loki work together.**
Prometheus collects metrics, Grafana visualizes them, and Loki handles centralized log management.

**2. What kind of metrics would you monitor for a web application?**
CPU/memory usage, request latency, error rates, and pod restarts.

**3. How do you set up alerts for system failures?**
Configured alerting rules in Prometheus and linked them to Alertmanager or email notifications.

---

## 🐍 Section 7: Python for DevOps

**1. How do you use Python in your DevOps workflow?**
Used for writing small automation scripts, parsing logs, and integrating API-based operations.

**2. Can you write a Python script to check if a web service is running?**

```python
import requests
url = 'http://example.com'
response = requests.get(url)
if response.status_code == 200:
    print('Service is running')
else:
    print('Service down')
```

**3. Explain how Python can interact with cloud services.**
Using SDKs like boto3 (AWS) or google-cloud libraries for GCP.

**4. How can Python be used in Jenkins pipelines?**
By calling Python scripts in build stages for validation or automation.

---

## 🧾 Section 8: Scenario-Based Questions

**1. Terraform apply failed midway — how do you recover?**
Check Terraform logs, refresh the state, and re-run with `-auto-approve` after resolving conflicts.

**2. Jenkins pipeline stuck due to plugin conflict — how do you troubleshoot?**
Review Jenkins logs, disable problematic plugins, and restart Jenkins safely.

**3. Application container keeps restarting — what steps do you take?**
Check pod logs, describe events, verify health probes and resource limits.

---

## 💬 Section 9: HR & Behavioral

**1. Tell me about your most challenging automation project.**
Discuss your GCP Terraform + Jenkins + Ansible pipeline setup — focus on automation and problem-solving.

**2. How do you handle deployment failures under tight deadlines?**
Use rollback mechanisms and communicate proactively with the team.

**3. Where do you see yourself in 2 years as a DevOps engineer?**
Growing towards cloud-native architecture and advanced automation solutions.
