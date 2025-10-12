
# 🧩 Terraform Interview Revision Pack (2 Years Experience)

## 🎯 Quick Reference Cheat Sheet

### 🧠 Terraform Basics
- **Terraform:** IAC tool by HashiCorp (2014), automates infra using HCL.
- **Language:** HCL (HashiCorp Configuration Language).
- **Platform:** Cloud-agnostic (AWS, Azure, GCP, On-Prem).

### ⚙️ Core Workflow
| Command | Description |
|----------|--------------|
| `terraform init` | Initialize plugins & backend |
| `terraform validate` | Syntax check for configuration |
| `terraform plan` | Creates execution plan |
| `terraform apply` | Builds infra |
| `terraform destroy` | Destroys infra |

### 🗂️ State File
- **File:** `terraform.tfstate`
- **Purpose:** Tracks real resources in the cloud.
- **Backup:** `terraform.tfstate.backup`
- **Locking:** Done via S3 + DynamoDB to prevent corruption.

### 🔐 Backend Types
| Type | Access | Locking | Used For |
|------|----------|----------|-----------|
| Local | Single user | ❌ | Testing |
| Remote (S3, GCS) | Team | ✅ | Production |

### ⚙️ Variables
- Declared in `variables.tf`
- Environment: `TF_VAR_name=value`
- Multiple envs: `terraform.tfvars`, `dev.tfvars`, etc.
- CLI: `terraform apply -var="instance_type=t2.micro"`
- Sensitive: `sensitive = true`

### 🧱 Meta Arguments
| Argument | Purpose | Example |
|-----------|-----------|----------|
| `count` | Create identical resources | 3 EC2s |
| `for_each` | Create different configs | dev/test/prod |
| `depends_on` | Force order | S3 after EC2 |
| `lifecycle` | Manage behavior | prevent_destroy, ignore_changes |

### 🧩 Lifecycle Rules
| Rule | Purpose |
|------|----------|
| `prevent_destroy` | Protect resources |
| `ignore_changes` | Skip manual changes |
| `create_before_destroy` | Avoid downtime |

### 🧰 Provisioners
| Type | Runs On | Purpose |
|-------|----------|----------|
| `local-exec` | Local | Run command locally |
| `remote-exec` | Remote | Run command inside EC2 |
| `file` | Upload | Transfer script/file |

### 🧩 Modules
- Folder structure for reusable infra code.
- Root → calls → child modules.
- Each module has `main.tf`, `variables.tf`, `outputs.tf`.

### 🧠 Data Sources
- Fetch existing resources without creating new ones.
```hcl
data "aws_ami" "latest" {
  most_recent = true
  owners = ["amazon"]
  filter {
    name = "name"
    values = ["amzn2-ami-hvm-*"]
  }
}
```

### 🧩 Workspaces
| Command | Purpose |
|----------|----------|
| `terraform workspace new dev` | Create new environment |
| `terraform workspace list` | List workspaces |
| `terraform workspace select dev` | Switch workspace |
| `terraform workspace delete dev` | Delete workspace |

### 🧠 Terraform Import
```bash
terraform import aws_instance.myec2 i-0abc12345
```

### 🧰 Debugging & Logging
```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=logs.txt
```

### ⚡ Parallelism
```bash
terraform apply -parallelism=1
```

### 🧩 Terraform Refresh
- Syncs state with live infra.
```bash
terraform apply -refresh=false
```

### 🧱 Data Types
| Type | Example |
|------|----------|
| list | ["a","b"] |
| map | {Name="dev"} |
| set | ["a","b"] |

### 🌀 Dynamic Block
```hcl
dynamic "ingress" {
  for_each = [22, 80, 443]
  content {
    from_port = ingress.value
    to_port   = ingress.value
    protocol  = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

### 🧩 OpenTofu vs Terraform
| Feature | Terraform | OpenTofu |
|----------|------------|-----------|
| License | BSL | Open Source |
| Maintainer | IBM | Linux Foundation |
| Cloud | HCP | None |

### ☁️ Terraform Cloud (HCP)
- Remote state, cost estimation, Sentinel policies, collaboration.

### 🛡️ Sentinel Policy Example
```hcl
import "tfplan/v2"
main = rule {
  all tfplan.resources.aws_instance as _, i {
    all i as _, inst { inst.applied.tags["ENV"] is "PROD" }
  }
}
```

---

## 🎯 Rapid-Fire Q&A (20 Questions)

1️⃣ What language does Terraform use? → **HCL**  
2️⃣ Command to initialize providers? → **terraform init**  
3️⃣ Which file stores infra info? → **terraform.tfstate**  
4️⃣ How to lock state? → **Use S3 + DynamoDB**  
5️⃣ Command to validate syntax? → **terraform validate**  
6️⃣ What is `terraform fmt`? → **Formats Terraform files**  
7️⃣ Purpose of `plan -out`? → **Save plan for later apply**  
8️⃣ How to apply plan file? → **terraform apply myplan**  
9️⃣ How to skip approval? → **--auto-approve**  
10️⃣ How to mark variable as secure? → **sensitive = true**  
11️⃣ How to fetch existing resource info? → **data block**  
12️⃣ How to import manual resources? → **terraform import**  
13️⃣ How to restore lost state? → **from backup or S3 version**  
14️⃣ How to isolate environments? → **Workspaces**  
15️⃣ Command to delete specific resource? → **terraform destroy -target**  
16️⃣ How to debug? → **TF_LOG=TRACE**  
17️⃣ Difference between list & set? → **Set has no duplicates**  
18️⃣ What is parallelism default? → **10**  
19️⃣ What’s Terraform Cloud used for? → **Team collaboration & remote state**  
20️⃣ What’s OpenTofu? → **Open-source Terraform fork**  

---

### 🏁 Final Tip:
> Always back up your state file, pin provider versions, and test with `plan` before `apply`.
