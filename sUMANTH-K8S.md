📘 Kubernetes Interview Questions – Extracted (Questions Only)
Ready to use for self-practice or mock interviews
1️⃣ Kubernetes Fundamentals
1. What is Kubernetes and why is it used?
2. What problems does Kubernetes solve that Docker alone cannot?
3. Explain Kubernetes architecture at a high level.
4. What is a cluster in Kubernetes?
5. Difference between containerization vs orchestration?
2️⃣ Kubernetes Architecture (Control Plane & Nodes)
6. Explain the role of:

API Server

etcd

Scheduler

Controller Manager

7. What happens if etcd goes down?
8. What is a node?
9. What components run on a worker node?
10. What is kubelet?
3️⃣ Pods & Containers
11. What is a Pod?
12. Why is Pod the smallest deployable unit?
13. Can a Pod have multiple containers? When would you do that?
14. What happens when a Pod dies?
15. Difference between:

Pod vs Deployment

Pod vs ReplicaSet

4️⃣ Workloads
16. What is a Deployment?
17. Difference between:

Deployment vs StatefulSet

Deployment vs DaemonSet

DaemonSet vs Job

Job vs CronJob

18. What is a ReplicaSet?
19. How does Kubernetes maintain desired state?
5️⃣ Services & Networking
20. What is a Service?
21. Explain Service types:

ClusterIP

NodePort

LoadBalancer

ExternalName

22. Difference between ClusterIP vs NodePort
23. How does Pod-to-Pod communication work?
24. What is kube-proxy?
25. How does Kubernetes DNS work?
6️⃣ Ingress & Traffic Management
26. What is Ingress?
27. Difference between Service vs Ingress
28. What is an Ingress Controller?
29. Name popular Ingress controllers.
30. How do you expose multiple services with one LoadBalancer?
7️⃣ Config & Secrets
31. What is a ConfigMap?
32. What is a Secret?
33. Difference between ConfigMap vs Secret
34. How do you mount secrets into pods?
35. Why should secrets not be stored in Git?
8️⃣ Storage
36. What is a Volume?
37. Difference between emptyDir vs hostPath
38. What is a PersistentVolume (PV)?
39. What is a PersistentVolumeClaim (PVC)?
40. Difference between PV vs PVC
41. What is a StorageClass?
9️⃣ Scheduling & Scaling
42. What is the Kubernetes scheduler?
43. What is HPA (Horizontal Pod Autoscaler)?
44. What metrics does HPA use?
45. Difference between HPA vs VPA
46. What is Cluster Autoscaler?
🔟 Security & Access Control
47. What is RBAC?
48. Difference between Role vs ClusterRole
49. What is a ServiceAccount?
50. What is PodSecurity (PSA)?
51. How do you secure a Kubernetes cluster?
🔥 Scenario-Based Questions
52. Your pod is in CrashLoopBackOff. How do you debug it?
53. Pod is running but application is not accessible externally. How do you debug?
54. How do you deploy a new version without downtime?
55. Pods are stuck in Pending state. Why?
56. ConfigMap updated but pods didn’t pick up changes — why?
57. How do you manage secrets securely in production?
58. A namespace consumes too many resources. How do you control it?
59. A node goes down. What happens to pods?
60. How do you upgrade a Kubernetes cluster safely?
61. How do you manage Dev, QA, and Prod environments in Kubernetes?
🔷 Advanced Kubernetes Questions
62. What is etcd quorum?
63. How does Kubernetes achieve self-healing?
64. What are Custom Resource Definitions (CRDs)?
65. What is an Operator?
66. What is a Service Mesh?
67. How is Kubernetes networking different from Docker networking?
68. How do you debug DNS issues in Kubernetes?
69. What is an Admission Controller?
70. What is a Pod Disruption Budget (PDB)?
71. What is OOS (Out of Sync) in Kubernetes?
72. What is QoS in Kubernetes?
📌 Extra: DNS & Headless Services
73. What is Kubernetes DNS?
74. What is a Headless Service?
75. What DNS records does Kubernetes use (A, AAAA, CNAME)?
