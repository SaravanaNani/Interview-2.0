
# ⚙️ Jenkins Interview Revision Pack (For 2 Years DevOps Experience)

## 🧠 Jenkins Overview
- Jenkins is an **open-source CI/CD automation server** written in Java.
- It helps automate build, test, and deployment pipelines.
- Supports **plugins** for integration with Git, SonarQube, Docker, Terraform, Ansible, etc.

---

## 🧩 Core Concepts

### 🔹 CI/CD
- **CI (Continuous Integration)** → Build & test code automatically on commits.  
- **CD (Continuous Delivery/Deployment)** → Automatically deliver or deploy code after testing.

**Common CI/CD Tools:** Jenkins, GitHub Actions, GitLab CI, CircleCI, Azure DevOps.

---

### 🔹 Jenkins Architecture
| Component | Description |
|------------|--------------|
| **Controller (Master)** | Manages jobs, UI, and scheduling |
| **Agent (Node/Slave)** | Executes build tasks |
| **Executor** | Slot on agent to run builds |
| **Workspace** | Directory where Jenkins stores job files during builds |

---

### 🔹 Jenkins Agent / Node
- Agents can be **physical**, **virtual**, or **container-based (Docker)**.
- Offloads workload from master and allows **parallel & isolated builds**.
- Docker agents provide lightweight, consistent environments.

```groovy
pipeline {
  agent { docker { image 'maven:3.8.7-jdk-11' } }
  stages {
    stage('Build') {
      steps { sh 'mvn clean package' }
    }
  }
}
```

---

### 🔹 Jenkins Pipeline Types
| Type | Syntax | Use Case |
|------|---------|----------|
| **Declarative** | Structured syntax (`pipeline {}`) | Simpler, predictable workflows |
| **Scripted** | Groovy-based (`node {}`) | Dynamic logic, advanced automation |

**Example (Declarative):**
```groovy
pipeline {
  agent any
  stages {
    stage('Build') { steps { sh 'mvn clean package' } }
    stage('Deploy') { steps { sh 'ansible-playbook deploy.yaml' } }
  }
}
```

**Example (Scripted):**
```groovy
node {
  stage('Build') { sh 'mvn clean package' }
  stage('Deploy') { sh 'ansible-playbook deploy.yaml' }
}
```

---

### 🔹 SCM Integration
- Connect to Git via **Source Code Management (SCM)** section.  
- Provide **Git URL + Credentials (token/SSH)**.  
- Enable **webhooks** to auto-trigger Jenkins builds on commits or PRs.

**Multibranch Pipeline** → Automatically detects all Git branches and builds them separately.

---

### 🔹 Build Triggers
| Trigger Type | Description |
|---------------|--------------|
| **Webhook** | Git pushes trigger Jenkins instantly |
| **Poll SCM** | Checks repo periodically (`H/5 * * * *`) |
| **Manual/Parameterized** | Trigger job with user input |

---

### 🔹 Credentials & Secrets Management
- Store sensitive data in **Jenkins Credentials Plugin**.
- Types: Username/Password, Secret Text, SSH Key, AWS Keys.
- Use in pipeline via `withCredentials()` block.

```groovy
withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]) {
  sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
}
```

- Supports **Vault / GCP Secret Manager** for external secret management.

---

### 🔹 Jenkins + Terraform + Ansible Integration
**Terraform Stage:**
```groovy
stage('Terraform Apply') {
  when { expression { params.ACTION == 'apply' } }
  steps {
    input message: 'Approve Terraform Apply?'
    sh 'terraform apply -auto-approve'
  }
}
```

**Ansible Stage:**
```groovy
stage('Ansible Configuration') {
  steps {
    sh 'ansible-playbook -i inventory apply.yaml'
  }
}
```

- Parameterized builds to **skip or apply** Terraform.  
- Email approvals before **apply/destroy** actions.

---

### 🔹 Jenkins + Docker Integration
- Use Docker agent to isolate builds.
- Run Docker commands inside pipelines to build/push images.

```groovy
stage('Build Docker Image') {
  steps {
    sh 'docker build -t app:${BUILD_NUMBER} .'
    sh 'docker push repo/app:${BUILD_NUMBER}'
  }
}
```

---

### 🔹 Troubleshooting Build Failures
1. Check **Console Output** for errors.  
2. Validate artifact from **Nexus**.  
3. Verify Jenkins → Target VM connectivity.  
4. Confirm Tomcat or service status.  
5. Use `cleanWs()` to clear workspace.  
6. Check permission or credential issues.

---

### 🔹 Best Practices
- Use version-controlled Jenkinsfiles.  
- Clean workspaces (`cleanWs()`) between builds.  
- Use credentials plugin for secrets.  
- Modularize pipelines (Build, Test, Deploy).  
- Add retries and approval gates.  
- Enable email/slack alerts for success/failure.  
- Use agents (Docker/slave nodes) for load balancing.

---

## 🎯 Rapid-Fire Jenkins Q&A (20 Questions)

1️⃣ What is Jenkins? → CI/CD automation server.  
2️⃣ What are Jenkins agents? → Nodes where jobs execute.  
3️⃣ What’s a Jenkins workspace? → Directory for build files/logs.  
4️⃣ Difference between Declarative and Scripted? → Structure vs Logic flexibility.  
5️⃣ How does Jenkins connect to Git? → SCM config + credentials.  
6️⃣ What triggers a Jenkins build? → Webhooks, Poll SCM, Manual.  
7️⃣ How do you handle secrets? → Credentials plugin / Vault.  
8️⃣ How do you debug a failing pipeline? → Console logs, check infra, workspace.  
9️⃣ What is `cleanWs()` used for? → Clean job workspace.  
10️⃣ How to approve Terraform stages? → Use `input` step or email-ext plugin.  
11️⃣ Jenkins + Docker integration? → Use Docker agents or build images in pipeline.  
12️⃣ How do you handle parallel jobs? → Multiple agents or parallel blocks.  
13️⃣ What is Multibranch Pipeline? → Auto-build branches detected from Git.  
14️⃣ How to store artifacts? → Nexus or S3 integration.  
15️⃣ How to trigger build on commit? → GitHub Webhook.  
16️⃣ How to retry a failed step? → `retry(3)` block in pipeline.  
17️⃣ How to send build notifications? → Email-ext or Slack plugin.  
18️⃣ How to prevent manual deployment errors? → Use approvals & automation gates.  
19️⃣ What is Jenkinsfile? → Code defining pipeline stages.  
20️⃣ Why use Docker agents? → Lightweight, isolated, reusable build environments.

---

### 🏁 Final Tips
> ✅ Always use parameterized pipelines for flexibility.  
> ✅ Secure credentials and use Vault for production.  
> ✅ Integrate Terraform + Ansible for end-to-end automation.  
> ✅ Review console logs first for any failure.  
> ✅ Modularize and document every Jenkinsfile.

