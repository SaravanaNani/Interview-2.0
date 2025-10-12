
# ☁️ GCP Complete Interview Guide (Core + Terraform + Automation)

This guide combines **core GCP concepts** with **Terraform and Jenkins automation**, reflecting real project scenarios for end-to-end DevOps cloud provisioning.

---

## 🌤️ GCP Set 1: Core Cloud Concepts

### 🧩 Q1. What are some core services provided by GCP, and which ones did you use in your project?

**Core Categories:**
- **Compute:** Compute Engine (VMs), GKE (Kubernetes Engine), Cloud Functions  
- **Storage:** Cloud Storage, Filestore, Persistent Disks  
- **Networking:** VPC, Load Balancer, Cloud DNS, Cloud NAT  
- **Database:** Cloud SQL, Bigtable, Firestore  
- **IAM & Security:** IAM Roles, Service Accounts, Secrets Manager

**Project Use:**
> Used Compute Engine, VPC, Subnets, and Cloud Storage for deploying an internal web app (“PC Listener”). IAM controlled access, and Terraform automated VM and network provisioning.

---

### 🧩 Q2. What is IAM in GCP, and how do roles and service accounts differ?

**IAM** manages access control through policies, roles, and members.

| Component | Description |
|------------|--------------|
| **User/Member** | Identity (user, group, or SA) |
| **Role** | Set of permissions (Viewer, Editor, Owner, Custom) |
| **Service Account** | Identity for applications/VMs to access GCP APIs |

**Example:**
> Terraform SA with roles `compute.admin`, `storage.admin` used for automated provisioning and deployment in Jenkins.

---

### 🧩 Q3. What is a VPC in GCP, and what are its components?

> A **VPC** (Virtual Private Cloud) provides an isolated virtual network environment.

**Components:**
- **Subnets** (regional CIDR blocks)
- **Firewall Rules** (traffic control)
- **Routes** (internal/external traffic)
- **Peering** (connect VPCs privately)
- **Shared VPC** (cross-project access)

**Example:**
> Custom VPC for Jenkins and Tomcat servers (public + private subnets). Managed entirely by Terraform.

---

### 🧩 Q4. Explain Subnets, Firewall Rules, and Routes in a GCP VPC.

| Component | Purpose | Example |
|------------|----------|----------|
| **Subnet** | Defines IP range within a VPC | `10.0.0.0/24` |
| **Firewall Rule** | Controls ingress/egress traffic | Allow TCP:22,80,443 |
| **Route** | Directs traffic within/between networks | Default route → Internet gateway |

**Analogy:** Subnets = “Neighborhoods,” Firewalls = “Security Guards,” Routes = “Roads.”

---

### 🧩 Q5. What is the purpose of Service Accounts in GCP, and how did you use them?

> Service Accounts (SAs) are special identities used by applications, VMs, or Terraform to access GCP securely.

**Example:**
```bash
gcloud iam service-accounts create terraform-sa   --display-name="Terraform Automation SA"

gcloud projects add-iam-policy-binding my-project   --member="serviceAccount:terraform-sa@my-project.iam.gserviceaccount.com"   --role="roles/compute.admin"
```
**Use:**  
- Terraform authenticated via SA JSON key.  
- Jenkins used SA to deploy infrastructure.  

---

## ⚙️ GCP Set 2: Terraform + Automation

### 🧩 Q1. How do you configure Terraform with GCP?

**Provider Configuration:**
```hcl
provider "google" {
  credentials = file("gcp-key.json")
  project     = "my-gcp-project"
  region      = "us-central1"
  zone        = "us-central1-a"
}
```
**Commands:**
```bash
terraform init
terraform plan
terraform apply -auto-approve
```

**Project:** Terraform provisioned Compute Engine, VPCs, and Cloud Storage.  

---

### 🧩 Q2. How do you store Terraform state securely in GCP?

**Backend Configuration:**
```hcl
terraform {
  backend "gcs" {
    bucket = "terraform-state-bucket"
    prefix = "env/dev"
  }
}
```
**Benefits:**
- Centralized state file  
- State locking and versioning  
- Team collaboration  

**Project:** Used GCS bucket with versioning enabled for global state management.

---

### 🧩 Q3. How to manage IAM roles and Service Accounts via Terraform?

```hcl
resource "google_service_account" "terraform_sa" {
  account_id   = "terraform-sa"
  display_name = "Terraform Automation SA"
}

resource "google_project_iam_member" "binding" {
  project = "my-gcp-project"
  role    = "roles/editor"
  member  = "serviceAccount:${google_service_account.terraform_sa.email}"
}
```

**Roles Used:**
- `roles/compute.admin`
- `roles/iam.serviceAccountUser`
- `roles/storage.admin`

---

### 🧩 Q4. Automating Networking: VPC, Subnets, Firewalls

```hcl
resource "google_compute_network" "vpc" {
  name                    = "dev-vpc"
  auto_create_subnetworks = false
}

resource "google_compute_subnetwork" "subnet" {
  name          = "dev-subnet"
  ip_cidr_range = "10.0.1.0/24"
  region        = "us-central1"
  network       = google_compute_network.vpc.id
}

resource "google_compute_firewall" "allow_http" {
  name    = "allow-http"
  network = google_compute_network.vpc.name
  allow {
    protocol = "tcp"
    ports    = ["80", "8080", "22"]
  }
  source_ranges = ["0.0.0.0/0"]
}
```

**Project:** Separate modules for dev/prod with dedicated `.tfvars` files.

---

### 🧩 Q5. Compute Instance Automation

```hcl
resource "google_compute_instance" "tomcat_vm" {
  name         = "tomcat-server"
  machine_type = "e2-medium"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network    = google_compute_network.vpc.name
    subnetwork = google_compute_subnetwork.subnet.name
    access_config {}
  }

  service_account {
    email  = google_service_account.terraform_sa.email
    scopes = ["cloud-platform"]
  }

  metadata_startup_script = file("setup-tomcat.sh")
}
```

**Project:** Automatically installed Java and Tomcat using startup script.

---

### 🧩 Q6. Jenkins + Terraform Integration

**Pipeline Example:**
```groovy
pipeline {
  agent any
  parameters {
    choice(name: 'ACTION', choices: ['apply', 'destroy', 'skip'])
  }
  stages {
    stage('Init') { steps { sh 'terraform init' } }
    stage('Plan') { steps { sh 'terraform plan -var-file=env/${ENV}.tfvars' } }
    stage('Apply/Destroy') {
      steps {
        script {
          if (params.ACTION == 'apply')
            sh 'terraform apply -auto-approve -var-file=env/${ENV}.tfvars'
          else if (params.ACTION == 'destroy')
            sh 'terraform destroy -auto-approve -var-file=env/${ENV}.tfvars'
        }
      }
    }
  }
}
```

**Project:** Jenkins pipeline triggered Terraform infra creation or destruction with email approvals.

---

### 🧩 Q7. Handling Dependencies in Terraform

```hcl
resource "google_compute_address" "ip" {
  name = "static-ip"
}

resource "google_compute_instance" "app" {
  name         = "app-vm"
  machine_type = "e2-medium"
  depends_on   = [google_compute_address.ip]
}
```

**Use:** Ensured VPC → Subnet → VM creation order.

---

### 🧩 Q8. Managing Multiple Environments

```bash
terraform workspace new dev
terraform workspace select dev
terraform apply -var-file=dev.tfvars
```

**dev.tfvars**
```hcl
machine_type = "e2-small"
region       = "us-central1"
```

**prod.tfvars**
```hcl
machine_type = "e2-medium"
region       = "us-central1"
```

**Project:** Dev and Prod isolated via workspaces, with separate GCS backend prefixes.

---

### 🧩 Q9. Terraform + Ansible Integration

**Execution Flow:**
```bash
terraform output inventory > inventory.ini
ansible-playbook -i inventory.ini configure.yml
```

**Project:**  
Terraform → Create infra (VMs, networks)  
Ansible → Install Tomcat, JDK, deploy WAR  

All triggered automatically through Jenkins pipeline.

---

### 🧩 Q10. GCP Security and Best Practices

- Least-privilege IAM roles only  
- Firewall restrictions for minimal ports  
- Versioning enabled on GCS backend  
- Separate service accounts for dev/prod  
- Secrets stored in Jenkins credentials or Ansible Vault  

---

## 🧠 Quick Revision Summary

| Concept | Description |
|----------|--------------|
| **Provider Setup** | GCP provider + SA key authentication |
| **Remote Backend** | GCS bucket with locking/versioning |
| **IAM via Terraform** | Automate SA creation and role binding |
| **VPC Automation** | Terraform-managed subnets, routes, firewalls |
| **Compute Automation** | VM provisioning with startup scripts |
| **Jenkins CI/CD** | Automated apply/destroy workflows |
| **Workspaces** | Dev/Prod environment isolation |
| **Terraform + Ansible** | Infra + Configuration synergy |
| **Security** | Least privilege, locked backend, encrypted keys |

---

**Prepared by:** Saravana L  
*(DevOps Engineer | GCP | Terraform | Jenkins | Ansible | EKS)*
