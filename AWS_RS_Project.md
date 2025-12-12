# 📘 DevOps Interview Q&A – Complete 50-Question Guide
Terraform • AWS • Ansible • Docker • Jenkins • CI/CD • Real-Time Scenarios

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
