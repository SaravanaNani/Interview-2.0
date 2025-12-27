# Ansible Interview Questions (Refined & Deduplicated)

## 1️⃣ Ansible Fundamentals
1. What is Ansible and why is it used?
2. What is agentless architecture in Ansible?
3. How does Ansible communicate with managed nodes?
4. What are Ansible modules?
5. What is the difference between Ansible and shell scripts?
6. What is the difference between Ansible and Puppet/Chef?

## 2️⃣ Ansible Architecture & Core Components
7. What is a control node in Ansible?
8. What are managed nodes?
9. What is an Ansible inventory?
10. What are the types of inventory (static vs dynamic)?
11. What is an Ansible collection?
12. What is the difference between a module and a plugin?

## 3️⃣ Playbooks, Tasks & YAML
13. What is an Ansible playbook?
14. What is the structure of an Ansible playbook?
15. What is the difference between a playbook, a play, and a task?
16. What is idempotency in Ansible?
17. How does Ansible ensure idempotency?

## 4️⃣ Variables & Facts
18. What are the different types of variables in Ansible?
19. What is variable precedence in Ansible (high level)?
20. What are Ansible facts?
21. What is ansible_facts?
22. How do you define custom facts?
23. What is the setup module and when is it used?

## 5️⃣ Roles & Reusability
24. What is an Ansible role?
25. What is the standard role directory structure?
26. What is the difference between a role and a playbook?
27. How do you share roles across projects?
28. What is the difference between import_role and include_role?
29. What is parse time vs runtime in Ansible?

## 6️⃣ Inventory & Environment Management
30. How do you group hosts in Ansible?
31. What are group_vars and host_vars?
32. How do you manage multiple environments (dev, QA, prod)?
33. What is a dynamic inventory and when would you use it?
34. How does Ansible fetch cloud inventories?

## 7️⃣ Handlers, Tags & Conditionals
35. What is a handler in Ansible?
36. When are handlers executed?
37. What are tags and why are they useful?
38. What is a when condition?
39. What is register and how is it used?

## 8️⃣ Templates & Files
40. What is Jinja2 in Ansible?
41. What is the difference between template and copy?
42. How do you loop in Ansible?
43. What is the difference between loop and with_items?
44. How do you manage configuration files dynamically?

## 9️⃣ Security & Vault
45. What is Ansible Vault?
46. How do you encrypt variables in Ansible?
47. How do you rotate secrets in Ansible?
48. Can Ansible Vault be integrated with CI/CD?
49. Why should secrets not be stored in plain YAML?

## 🔹 Scenario-Based Ansible Interview Questions
50. A playbook runs successfully but reports changes every time. Why?
51. An Ansible playbook is slow. How do you optimize it?
52. How do you structure Ansible for dev, QA, and prod environments?
53. How do you install software only on specific hosts?
54. How do you restart a service only when a configuration changes?
55. How do you prevent secrets from being exposed in logs?
56. Dynamic inventory is not fetching hosts. How do you debug it?
57. A playbook fails on one host but succeeds on others. How do you handle it?
58. How do you perform rolling deployments without downtime?
59. Servers drift from desired configuration. How do you fix and prevent it?

## 🔹 Advanced Ansible Questions
60. What is the difference between include_tasks and import_tasks?
61. What are Ansible callback plugins?
62. How does Ansible scale to thousands of nodes?
63. What is Ansible Tower / AWX?
64. How is RBAC implemented in Ansible Tower/AWX?
65. How do you test Ansible roles?
66. What is Molecule?
67. How do you manage Windows hosts in Ansible?
68. How do you debug complex Ansible playbooks?
69. How do you integrate Ansible with Terraform?
