# DevOps Self-Mock Interview Questions (82 Questions)
## Terraform • AWS • Jenkins • Ansible • Docker • CI/CD • Real-Time Scenarios

---

## 🔹 Core DevOps Questions (1–50)

1. What is Terraform, and why do we use it instead of manual provisioning?
2. What is Infrastructure as Code (IaC)?
3. How is Terraform different from AWS CloudFormation?
4. What is a Terraform state file, and why is it important?
5. What problems occur if Terraform state file is lost?
6. Why should we use a remote backend instead of a local backend?
7. How does Terraform remote backend help teams?
8. What is state locking in Terraform?
9. How does DynamoDB help with Terraform state locking?
10. What are Terraform modules and why do we use them?
11. How do Terraform modules improve reusability?
12. How do you structure Terraform code for multiple environments?
13. What are tfvars files and why are they used?
14. What is the difference between terraform plan and terraform apply?
15. Why should terraform plan and apply be separate stages in Jenkins?
16. What is Terraform drift?
17. How do you detect Terraform drift?
18. How do you fix Terraform drift?
19. What happens if an EC2 instance is deleted manually from AWS?
20. What is terraform import and when do you use it?
21. What is terraform taint?
22. What are Terraform provisioners?
23. What is the difference between local-exec and remote-exec?
24. Why are Terraform provisioners not recommended?
25. What is the purpose of the .terraform.lock.hcl file?
26. Why should .terraform.lock.hcl be committed to Git?
27. What is a Docker image?
28. What is a Docker container?
29. What is the difference between Docker image and container?
30. What is a Docker bridge network?
31. What is the difference between COPY and ADD in Dockerfile?
32. What is the difference between Docker volume and bind mount?
33. Why are Docker volumes preferred in production?
34. What is a Jenkins pipeline?
35. Why do we use Docker agents in Jenkins?
36. What is the difference between Jenkins Docker agent and SSH agent?
37. Why should Terraform not be run directly from a developer laptop?
38. What is Ansible?
39. What is idempotency in Ansible?
40. What are Ansible roles and why are they used?
41. What is Ansible dynamic inventory?
42. How does Ansible connect to target hosts without passwords?
43. What is the difference between public and private subnet?
44. What is an Internet Gateway?
45. What is a NAT Gateway?
46. What is the difference between IGW and NAT Gateway?
47. What is a Bastion Host?
48. Why should private servers not be exposed to the internet?
49. What is an IAM Role?
50. Why is IAM Role preferred over IAM user access keys on EC2?

---

## 🔹 Experience & Real-Time Project Questions (51–82)

51. Explain your end-to-end CI/CD workflow using Jenkins, Maven, and Nexus.
52. How do you control artifact versioning in your pipeline?
53. How do you ensure the correct artifact is uploaded to Nexus?
54. How do you make sure the same artifact is deployed across environments?
55. How do you handle Maven build failures in Jenkins?
56. What kind of Terraform validation errors have you faced?
57. How do IAM permission issues affect Terraform runs?
58. How does Jenkins get AWS permissions without access keys?
59. How do you handle Nexus upload failures?
60. How do you prevent corrupted artifacts from being deployed?
61. How do you implement rollback in your deployment pipeline?
62. How does Jenkins BUILD_NUMBER help in versioning?
63. What happens if a Jenkins build fails between two successful builds?
64. How do you handle concurrent Jenkins builds?
65. How do you prevent two deployments from running at the same time?
66. How do you detect application issues after deployment?
67. What do you do if deployment succeeds but app is not accessible?
68. How do you decide between rollback and fix-forward?
69. How do you balance fast delivery with production stability?
70. How do you handle transient failures like network or Nexus downtime?
71. What is your role during a production incident?
72. How do you verify deployment success automatically?
73. How do you manage software version upgrades safely?
74. How do you handle automation changes breaking an environment?
75. How do you isolate Terraform state between teams and environments?
76. How do you validate infrastructure changes before production?
77. How do you manage secrets differently for prod and non-prod?
78. How do you prevent CI/CD pipeline changes from breaking teams?
79. How do you control infrastructure cost and avoid resource sprawl?
80. What best practices do you follow for maintainable Terraform & Ansible code?
81. How do you ensure auditability and traceability in CI/CD pipelines?
82. What improvement would you make if you start this project again?

---

## ✅ End of Self-Mock Question Set
