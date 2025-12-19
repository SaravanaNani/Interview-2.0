# Terraform – Interview Questions (Extracted Only)

## 1. Terraform Lifecycle Commands
1. What does `terraform fmt` do?
2. What does `terraform init` do?
3. What does `terraform validate` do?
4. What does `terraform plan` do?
5. When you run `terraform plan`, what does Terraform check and show?
6. What are the different sources of truth for terraform plan?
7. What does `terraform apply` do?
8. When is the `.tfstate` file created?
9. What does `terraform plan -out=tfplan` do?
10. What does `terraform refresh` do?

## 2. Terraform Internal Files
11. When is the `.terraform/` directory created?
12. What is inside the `.terraform/` directory?
13. When is `.terraform.lock.hcl` created?
14. What is inside `.terraform.lock.hcl`?
15. What files should or should not be committed to Git?

## 3. State Management & Locking
16. How do you unlock a Terraform lock?
17. What problems occur if the state file is lost or corrupted?
18. How does Terraform state locking work?
19. What happens if two users run `terraform apply` at the same time?
20. Explain `terraform state list`, `terraform state mv`, `terraform state rm`, and `terraform state pull`.

## 4. Providers & Versions
21. Can you use multiple providers in one Terraform project?
22. What happens if a provider version changes?

## 5. Variables & Modules
23. Difference between `terraform.tfvars` and `.auto.tfvars`.
24. Difference between root module and child module.
25. How do you version Terraform modules?
26. What are module inputs and outputs?
27. When should you not use modules?

## 6. Meta Arguments
28. Difference between `count` and `for_each`.
29. Explain dynamic blocks.
30. What are Terraform functions?
31. What is the lifecycle block?
32. Explain:
    - `create_before_destroy`
    - `prevent_destroy`
    - `ignore_changes`
    - `replace_triggered_by`

## 7. Workspaces & Environments
33. Difference between workspaces and separate state files.
34. Do you recommend workspaces for production? Why or why not?

## 8. Security & Scanning
35. How do you scan Terraform code for security issues?

## 9. Infrastructure Drift & External Changes
36. What is Terraform drift?
37. What happens if a resource is manually deleted from the cloud console?

## 10. Scenario-Based Questions
38. Two engineers ran `terraform apply` at the same time and state got corrupted — how do you prevent and fix it?
39. A resource was manually deleted — how do you detect drift and re-create or ignore it?
40. You renamed a resource and Terraform wants to recreate it — how do you avoid downtime?
41. How to structure Terraform for multiple environments (dev/qa/prod)?
42. How to update infrastructure without downtime using Terraform?
43. How to run Terraform safely in a CI/CD pipeline?
44. How to design a reusable module (e.g., VPC module)?
45. After upgrading a provider version, many changes appear — what do you do?
46. Terraform apply takes 45 minutes — how do you optimize it?
47. How do you manage secrets in Terraform?
48. How do you refactor Terraform without downtime?
49. How do you manage cross-account / cross-subscription deployments?

## 11. Taint & Replacement
50. What replaced `terraform taint`?
51. When should you force-replace a resource?
52. How do you avoid downtime during resource replacement?

## 12. Miscellaneous Terraform Behavior
53. You added a new port to a security group — what happens when you run apply?
54. What is the Terraform console used for?
55. What are Terraform meta-arguments?
56. What are provisioners?
57. What is a data block (data source) in Terraform?
58. How does state locking work with DynamoDB?

## 13. Dependencies
59. What is implicit dependency in Terraform?
60. What is explicit dependency in Terraform?
