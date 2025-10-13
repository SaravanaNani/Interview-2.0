
# ☁️ GCP Service Account Impersonation – Short Interview Summary

This guide summarizes all 3 impersonation methods with simple 2–3 line explanations — ideal for interviews.

---

## 🔹 Overview
- A **Service Account (SA)** is both an *identity* (email) and a *resource* (can have roles & policies).
- Used when applications, VMs, or users need to authenticate securely to access GCP services.

---

## 🧩 Method 1: Deploying Jobs as a Service Account (Compute/App/Functions)

- Resources like Compute Engine, App Engine, or Cloud Functions run as a **Service Account identity**.
- The user must have **Service Account User (`iam.serviceAccountUser`)** permission to deploy or execute as that SA.
- ✅ *Used when a VM or GCP job needs to access other services like GCS or BigQuery internally.*

---

## 🧩 Method 2: Authentication Using Private Key (Key File JSON)

- External tools or users can act as a Service Account by authenticating via the **private key file**.
- Command:  
  ```bash
  gcloud auth activate-service-account --key-file=key.json
  ```
- ✅ *Used for automation via Jenkins, GitLab CI, or local CLI outside GCP.*
- 🔒 Keys should be rotated regularly; avoid long-lived keys for better security.

---

## 🧩 Method 3: Using IAM Policy Impersonation (Token Creator Role)

- A user temporarily impersonates a Service Account using the flag:
  ```bash
  gcloud compute instances list --impersonate-service-account=<SA_EMAIL>
  ```
- Requires the **Service Account Token Creator (`roles/iam.serviceAccountTokenCreator`)** role.
- ✅ *Used for secure, auditable, and time-limited impersonation without sharing keys.*

---

## ✅ Key Best Practices

| Practice | Description |
|-----------|--------------|
| **Least Privilege** | Grant only required roles (avoid Owner/Editor) |
| **Avoid Default SAs** | Create custom SAs for specific workloads |
| **Rotate Keys** | Replace & delete old keys regularly |
| **Grant Roles to Groups** | Easier management than per-user IAM roles |
| **Use IAM Conditions** | Apply time-based or resource-based access limits |
| **Monitor via Audit Logs** | Track who impersonated which SA and when |

---

**Prepared by:** Saravana L  
*(DevOps Engineer | GCP | IAM | Terraform | Jenkins)*
