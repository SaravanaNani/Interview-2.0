# 📘 DevOps Interview Q&A – Complete 50-Question Guide

# DevOps Real-Time Interview Questions & Answers  
## (Based on CI/CD, Jenkins, Terraform, Ansible, Docker, AWS – Experience Level)

---

## Q1. Explain your end-to-end CI/CD workflow using Jenkins, Maven, and Nexus.
**Answer:**  
We used Jenkins to automate the CI/CD pipeline where Maven builds the Java application and generates a versioned artifact using the Jenkins BUILD_NUMBER. After validation and tests, the artifact is uploaded to Nexus. Deployment stages then pull the same artifact from Nexus and deploy it to the target servers.

---

## Q2. How did you automate log checks, health checks, and deployment validation?
**Answer:**  
We used simple Python/Bash scripts to check application logs and hit health endpoints after deployment. If the health check returned non-200 status or logs showed errors, the pipeline failed immediately to prevent bad deployments.

---

## Q3. How do you troubleshoot Maven build failures in Jenkins?
**Answer:**  
We check Jenkins console logs to identify dependency issues, compilation errors, or test failures. Infrastructure-related issues are fixed in Jenkins or pom.xml, while code-related issues are shared with the development team for resolution.

---

## Q4. What Terraform validation or IAM issues did you face?
**Answer:**  
We faced issues like variable name mismatches, incorrect resource references, and IAM permission errors. These were fixed using `terraform validate`, consistent naming, lifecycle rules like `create_before_destroy`, and attaching proper IAM roles to the Jenkins VM.

---

## Q5. How does Jenkins get AWS permissions without access keys?
**Answer:**  
Jenkins runs on an EC2 VM that has an IAM Role attached. All Jenkins jobs and Docker agents inherit the role’s permissions automatically using the instance metadata service, so no access keys are required.

---

## Q6. How do you handle Nexus upload failures or corrupted artifacts?
**Answer:**  
We check Jenkins logs for credential, network, or permission issues. Before uploading artifacts, we ensure build success, run tests, and static analysis. Only validated artifacts are uploaded and deployed from Nexus.

---

## Q7. How do you implement rollback in your deployment pipeline?
**Answer:**  
Each build produces a unique artifact version stored in Nexus. If deployment fails, we redeploy the last successful artifact version from Nexus to restore the application quickly.

---

## Q8. How do you ensure the same artifact is deployed across environments?
**Answer:**  
We follow a “build once, deploy many” approach. The artifact is built once, uploaded to Nexus, and the same version is deployed to QA and Prod using the version passed through pipeline parameters.

---

## Q9. How do you handle concurrent Jenkins builds?
**Answer:**  
We restrict concurrent builds using pipeline configuration or environment locks so only one deployment runs at a time, preventing conflicts and accidental overwrites.

---

## Q10. What if a deployment succeeds but the app is slow or unstable?
**Answer:**  
We monitor health checks, logs, and basic metrics. If users are impacted, we rollback to the last stable version first, then analyze logs and metrics to identify the root cause.

---

## Q11. What documentation did you maintain?
**Answer:**  
We documented infrastructure folder structure, Terraform modules, Jenkins pipelines, Ansible roles, common errors, resolutions, and future upgrade steps like Tomcat or Java version changes.

---

## Q12. How did you improve provisioning efficiency by ~35%?
**Answer:**  
Manual setup earlier took 4–5 hours. By automating infrastructure with Terraform, configuration with Ansible, and orchestration using Jenkins, the entire setup reduced to 15–20 minutes.

---

## Q13. How do you troubleshoot a slow Jenkins pipeline?
**Answer:**  
We check Jenkins VM CPU, memory, and disk usage. If overloaded, we move builds to Docker agents and optimize slow stages like dependency downloads or artifact uploads.

---

## Q14. How do you design reusable Jenkins pipelines?
**Answer:**  
We parameterized pipelines to accept environment, action, and version inputs. Terraform modules and Ansible roles were reused across environments using different variable files.

---

## Q15. How do you ensure minimal downtime during deployment?
**Answer:**  
We used automated deployments with quick stop-deploy-start strategy, health checks, and immediate rollback if validation failed. For future improvement, blue-green deployment was planned.

---

## Q16. How do you handle configuration changes without rebuilding artifacts?
**Answer:**  
Configuration is externalized using Jenkins environment variables, credentials, and Ansible templates. This allows changes without rebuilding the application.

---

## Q17. How do you secure CI/CD pipelines?
**Answer:**  
Secrets are stored in Jenkins Credentials, IAM roles are used instead of keys, production deployments require approvals, and access is restricted via RBAC.

---

## Q18. Pipeline succeeded but app is not accessible. What do you do?
**Answer:**  
We check health endpoints and logs first. If the issue persists, we rollback. Then we inspect infrastructure, ports, firewall rules, and load balancer settings.

---

## Q19. Rollback vs fix-forward – how do you decide?
**Answer:**  
If users are impacted or the root cause is unclear, we rollback immediately. If the issue is minor and well-understood, we fix forward with a new deployment.

---

## Q20. How do you balance stability and fast delivery?
**Answer:**  
Automation provides speed, while approvals, plan reviews, health checks, and rollback mechanisms ensure stability.

---

## Q21. How do you handle transient failures in Jenkins pipelines?
**Answer:**  
We design pipelines to fail fast, retry temporary issues, and prevent partial deployments by isolating stages and validations.

---

## Q22. What is your role during a production incident?
**Answer:**  
Acknowledge the alert, assess impact, rollback if needed, stabilize production, then analyze root cause and document the incident.

---

## Q23. How do you verify deployment success?
**Answer:**  
We use health checks returning HTTP 200, log validation, service status checks, and Jenkins stage success as indicators.

---

## Q24. How do you manage version upgrades safely?
**Answer:**  
Versions are controlled using Ansible variables and inventory files. Upgrades are tested in lower environments before production rollout.

---

## Q25. How do you handle automation changes breaking environments?
**Answer:**  
Issues are detected via logs and health checks, fixed quickly, tested in lower environments, and documented to avoid recurrence.

---

## Q26. How do you prevent cross-team or cross-env impact?
**Answer:**  
Each environment/team uses a separate Terraform remote backend and state file, while sharing the same reusable modules.

---

## Q27. How do you validate infra changes before production?
**Answer:**  
We run `terraform plan`, review changes, and require manual approval before `terraform apply`.

---

## Q28. How do you handle secrets in prod vs non-prod?
**Answer:**  
Different Jenkins credential IDs are used per environment. Production credentials have stricter access and approvals.

---

## Q29. How do you ensure pipeline changes don’t break teams?
**Answer:**  
Pipelines are version-controlled in Git, tested in non-prod jobs, and rolled back easily if issues occur.

---

## Q30. How do you control infrastructure cost?
**Answer:**  
We review Terraform plans, tag resources, destroy unused environments, and avoid manual resource creation.

---

## Q31. How do you ensure code maintainability?
**Answer:**  
Terraform modules, Ansible roles, clean variable separation, naming conventions, and documentation ensure maintainability.

---

## Q32. What improvement would you make if starting again?
**Answer:**  
Implement blue-green deployments with auto-scaling groups to achieve zero downtime and better scalability.

---

## ✅ End of Document


# Terraform • AWS • Ansible • Docker • Jenkins • CI/CD • Real-Time Scenarios

### 1. What is Terraform, and why do we use it?

Answer:
Terraform is an IaC tool that automates infrastructure creation using code, reduces manual work, and keeps track of resources using a state file. It supports multiple cloud providers, unlike CloudFormation which works only for AWS.

2. What is a Terraform state file and why is it important?

Answer:
The state file stores the real infrastructure details. Without it, Terraform cannot plan or update resources correctly.

3. Why should we use a remote backend instead of local state?

Answer:
Remote backend provides backup, versioning, and locking so teams can work safely. Local state can be lost or corrupted.

4. What are Terraform modules and why use them?

Answer:
Modules are reusable components. They avoid code duplication and ensure consistency across environments.

5. How do you integrate Terraform safely in Jenkins?

Answer:
Use init → plan → apply, remote backend (S3 + DynamoDB), approvals, tfvars, and Docker agent with fixed versions.

6. How do you pass Terraform outputs to Ansible?

Answer:
Export outputs as JSON and use them as Ansible inventory, or use Ansible dynamic inventory to auto-fetch hosts.

7. Why should Ansible run only after Terraform?

Answer:
Terraform creates servers; Ansible configures them. Configuration cannot run without infrastructure.

8. Docker Agent vs SSH Agent in Jenkins

Answer:
Docker agents are temporary and clean; SSH agents are permanent and risk version drift.

9. How to keep Terraform & Ansible versions consistent?

Answer:
Fix versions in Docker image, commit lockfiles, and run version checks in CI.

10. Why use Docker agents for Terraform & Ansible?

Answer:
They reduce load on Jenkins, give clean environments, and eliminate dependency issues.

11. Difference between Terraform plan and apply

Answer:
Plan shows changes; apply performs them. Plan helps avoid mistakes.

12. What is a Terraform backend?

Answer:
A backend is where the state file is stored (local or remote).

13. What happens if the Terraform state file is corrupted?

Answer:
Terraform cannot track resources. Restore the state from remote backend versioning.

14. What is dynamic inventory in Ansible?

Answer:
It automatically fetches cloud servers using tags.

15. Roles vs Playbooks in Ansible

Answer:
Playbook runs tasks. Roles organize tasks in reusable format.

16. What is idempotency in Ansible?

Answer:
Running playbooks multiple times will not change already-correct configurations.

17. Public subnet vs private subnet

Answer:
Public subnet has IGW access. Private subnet uses NAT for outbound-only internet.

18. NAT Gateway vs Internet Gateway

Answer:
IGW = full internet access. NAT = outbound-only internet for private subnets.

19. How Terraform outputs are passed to Ansible

Answer:
Generate JSON from Terraform outputs → Ansible inventory.

20. What if Ansible fails after Terraform succeeds?

Answer:
Fix the playbook and rerun only Ansible. No need to reapply Terraform.

21. Why use approval before Terraform apply?

Answer:
To avoid accidental production changes.

22. Why separate plan and apply in Jenkins?

Answer:
Better visibility, approval checks, and safer changes.

23. What happens if two people run Terraform at the same time?

Answer:
State corruption. DynamoDB locking prevents this.

24. How to structure Terraform modules for multiple environments?

Answer:
Use /environments/dev, /prod, /test each calling common modules.

25. How does Ansible help configure many servers?

Answer:
One playbook configures all servers at once.

26. Why use Dockerfiles?

Answer:
Automates image building; no manual installation.

27. Docker image vs container

Answer:
Image = blueprint; container = running instance.

28. What is a bridge network in Docker?

Answer:
Enables container-to-container communication on same host.

29. COPY vs ADD in Dockerfile

Answer:
COPY = simple copy; ADD = copy + untar + URL support.

30. Docker volume vs bind mount

Answer:
Volume = Docker-managed storage; bind mount = host folder mounted inside container.

31. Why prefer volumes in production?

Answer:
They are safer and more stable than bind mounts.

32. Security Group vs NACL

Answer:
SG = instance-level, allow-only. NACL = subnet-level, allow/deny.

33. IAM Role vs IAM User keys

Answer:
Roles are temporary and safe. User keys are permanent and risky.

34. What is a Bastion Host?

Answer:
A secure entry point to access private servers.

35. What happens if someone deletes an EC2 manually?

Answer:
Terraform plan shows recreate. Fix with apply, import, or state rm.

36. What is .terraform.lock.hcl?

Answer:
Locks provider versions for consistency; commit it to Git.

37. What is terraform taint?

Answer:
Marks a resource to recreate on next apply.

38. Local-exec vs remote-exec

Answer:
Local-exec runs on your machine; remote-exec runs inside the created server.

39. Why avoid provisioners?

Answer:
Unpredictable and break idempotency. Use Ansible instead.

40. What is Terraform drift?

Answer:
Infrastructure doesn’t match state file. Fix with apply, import, or state rm.

41. Why use Jenkins input steps?

Answer:
To pause for manual approval before risky actions.

42. What if plan shows unexpected changes?

Answer:
Stop pipeline and request approval.

43. Why split pipeline into multiple stages?

Answer:
Cleaner logs, safety, and approvals.

44. How to store sensitive credentials in Jenkins?

Answer:
Use Jenkins Credentials binding — never store in Git.

45. If Ansible succeeds on some hosts but fails on others?

Answer:
Check logs, fix the issue, rerun — idempotency covers it.

46. Why use lightweight Docker images?

Answer:
Faster builds, smaller size, better performance.

47. Why use variables & tfvars in modules?

Answer:
Reuse modules with different values across environments.

48. Why not run Terraform apply from a laptop?

Answer:
Unsafe, inconsistent, no audit, no locking.

49. Why Terraform first then Ansible?

Answer:
Infrastructure must exist before configuration.

50. What if apply fails halfway?

Answer:
Fix the error and rerun plan/apply — Terraform completes remaining resources.
