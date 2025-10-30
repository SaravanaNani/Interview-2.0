# Jenkins Pipeline Explanation & Interview Q&A

## 🧱 1️⃣ Infrastructure Pipeline (Terraform + Ansible)

### 💡 Simple Explanation

This Jenkins pipeline automates **infrastructure provisioning and configuration** on **Google Cloud (GCP)** using **Terraform** and **Ansible**. It includes manual approval gates to ensure safety before applying or destroying infrastructure.

#### Step-by-Step Overview

1. **Checkout:** Pulls Terraform and Ansible code from GitHub using the selected branch.
2. **Initial Check:** Ensures Terraform and Ansible stages aren’t both skipped.
3. **Get Credentials:** Fetches the GCP service account key from Jenkins credentials and configures Ansible.
4. **Terraform Plan & Apply:**

   * Runs `terraform plan` to preview infrastructure changes.
   * Sends email to the admin for approval before applying.
   * Applies Terraform to create or modify cloud resources.
   * Handles state checks and re-creation logic if resources already exist.
5. **Terraform Destroy:**

   * Sends email for manual approval before running `terraform destroy`.
   * Destroys infrastructure upon approval.
6. **Ansible Apply:** Executes playbooks to install and configure software (e.g., Java, Tomcat) on provisioned VMs.
7. **Ansible Destroy:** Removes or cleans configurations if necessary.

### ⚙️ Your Role (as per resume)

> Supported the Jenkins infrastructure pipeline that automated GCP provisioning using Terraform and configuration using Ansible. Tested Terraform modules, verified VM creation, updated Ansible playbooks for application setup, and ensured email approvals worked properly.

### 🧠 Common Interview Questions (Infra Pipeline)

#### HR-Level:

1. What is the purpose of your infrastructure pipeline?
2. Why did you include email approvals in Jenkins?
3. How does Terraform differ from Ansible?
4. What happens if both Terraform and Ansible are skipped?
5. What is the role of Terraform state?

#### Technical:

1. How does Jenkins authenticate to GCP?

   * *Using a service account key stored in Jenkins credentials.*
2. What does `terraform plan` do?

   * *It previews infrastructure changes before applying.*
3. How did you handle Terraform state management?

   * *By maintaining the state file in a backend and checking before apply.*
4. How do you trigger Ansible playbooks in Jenkins?

   * *By executing ansible-playbook commands inside Jenkins stages.*
5. What’s the purpose of the `emailext` and `input` steps?

   * *Used for manual approval and sending build notifications.*

---

## ☕ 2️⃣ Java Application Pipeline (CI/CD)

### 💡 Simple Explanation

This Jenkins pipeline automates **build, scan, upload, and deployment** of a **Java web application** to **Tomcat** running on a **GCP VM**.

#### Step-by-Step Overview

1. **Checkout:** Clones the Java project from GitHub.
2. **SonarQube Analysis:** Runs code quality analysis for bugs and vulnerabilities.
3. **Package:** Uses Maven to compile, test, and package the WAR file.
4. **Upload to Nexus:** Uploads the WAR to Nexus Repository with versioning (`1.2.${BUILD_NUMBER}`).
5. **Check Infrastructure:** Uses GCP CLI to verify if the target VM is running.
6. **Email Approval:** Sends an email to admin before deployment for confirmation.
7. **Deployment:**

   * Connects to the VM via SSH.
   * Stops Tomcat, removes old WAR files, uploads the new one, and restarts Tomcat.
8. **Update pom.xml:** Updates the project version in GitHub using build number.

### ⚙️ Your Role (as per resume)

> Supported and maintained the Java CI/CD pipeline for building, scanning, and deploying Java applications using Jenkins, Maven, SonarQube, Nexus, and Tomcat. Verified artifact uploads, ensured clean deployments, and assisted in version management via GitHub.

### 🧠 Common Interview Questions (Java Pipeline)

#### HR-Level:

1. Can you describe your Java pipeline end-to-end?
2. What is the purpose of SonarQube in your pipeline?
3. Why is Nexus used here?
4. How do you ensure deployment happens only when the VM is active?
5. How do you handle approvals before deployment?

#### Technical:

1. How does Jenkins connect to the target VM?

   * *Using SSH key credentials configured in Jenkins.*
2. What does `mvn clean package` do?

   * *It cleans previous builds, compiles, tests, and packages the app.*
3. How is rollback managed in case deployment fails?

   * *By redeploying the previous stable WAR version from Nexus.*
4. How is version management handled?

   * *By auto-updating the pom.xml version using the Jenkins build number.*
5. Why is `wget` used during deployment?

   * *To download the WAR artifact from Nexus repository.*

---

## 🔧 Combined Short Summary (for Interviews)

> I’ve worked on two main Jenkins pipelines — one for **infrastructure automation** using **Terraform and Ansible** on GCP, and another for **Java application CI/CD** using **Maven, SonarQube, Nexus, and Tomcat**. My responsibilities included verifying Terraform modules, testing Ansible playbooks, monitoring pipeline runs, ensuring successful deployments, and maintaining version control in GitHub.

---

## 📋 Quick Practice Q&A Summary

**Q:** What is the difference between Terraform and Ansible in your setup?
**A:** Terraform is used for provisioning infrastructure, while Ansible handles configuration and software deployment.

**Q:** How did you integrate approval stages in Jenkins?
**A:** By using the `input` step for manual confirmation and `emailext` for email notifications.

**Q:** Why did you use Nexus instead of just deploying directly from Jenkins?
**A:** Nexus acts as an artifact repository, enabling version control and rollback in case of deployment failure.

**Q:** How does SonarQube help in CI/CD?
**A:** It performs static code analysis to ensure code quality before packaging and deployment.

**Q:** What was your role during failures or troubleshooting?
**A:** I helped identify root causes in Jenkins logs, verified environment variables, checked build outputs, and coordinated fixes in Terraform or Ansible scripts.

---

## 🚀 Key Tools Used

* **Terraform & Ansible:** Infrastructure automation and configuration.
* **Jenkins:** Pipeline orchestration and automation.
* **SonarQube:** Code quality and security analysis.
* **Nexus:** Artifact storage and versioning.
* **Docker & Tomcat:** Application containerization and deployment.
* **GCP CLI:** Infrastructure verification and management.
* **Python/Shell:** Basic scripting and log checks.

---

This explanation and Q&A set will help you clearly explain your Jenkins pipelines and confidently answer both HR and technical DevOps interview questions.
