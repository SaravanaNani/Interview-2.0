# Docker – Interview Questions (Extracted Only)

## 1. Docker Fundamentals
1. What is Docker and why is it used?
2. Difference between Virtual Machines vs Containers?
3. What problem does Docker solve in DevOps?
4. What is containerization?
5. Explain Docker architecture.
6. What is Docker Engine?
7. Explain Docker Client, Docker Daemon, Docker Registry.
8. What is Docker Desktop vs Docker Engine?
9. What is the role of containerd?
10. How does Docker use Linux kernel features?
11. What is a Docker image?
12. What is a Docker container?
13. Difference between Image vs Container?
14. What happens when you run `docker run`?
15. What is an image layer?
16. Why are Docker images immutable?
17. What is a Dockerfile?
18. Common Dockerfile instructions? (FROM, RUN, CMD, ENTRYPOINT, COPY vs ADD)
19. Difference between CMD and ENTRYPOINT?
20. What is a multi-stage build?
21. Why should we avoid running containers as root?
22. What is .dockerignore and why is it important?
23. What are Docker network types? (Bridge, Host, None, Overlay)
24. Difference between Bridge vs Host network?
25. How do containers communicate with each other?
26. How does Docker DNS work?
27. What happens when you expose a port?
28. Difference between Volume, Bind mount, tmpfs?
29. When would you use Docker volumes?
30. Where are Docker volumes stored?
31. What happens to container data after container deletion?
32. How do you limit CPU and memory for a container?
33. What happens when a container exceeds memory limits?
34. What are Docker namespaces and cgroups?
35. How do you secure Docker containers?
36. What is Docker Content Trust?

## 2. Docker Compose
37. What is Docker Compose?
38. Difference between docker-compose vs docker run?
39. What is depends_on in Docker Compose?
40. Can Docker Compose be used in production?

## 3. Docker Registry & Distribution
41. What is Docker Hub?
42. Difference between public registry vs private registry?
43. How do you push images to a private registry?
44. How do you tag Docker images properly?

## 4. Docker Best Practices
45. How do you reduce Docker image size?
46. Why use Alpine images?
47. Why should we minimize layers?
48. How do you scan Docker images for vulnerabilities?

## 5. Scenario-Based Docker Questions
49. Container keeps crashing — how do you troubleshoot it?
50. Docker image size is too large — how do you reduce it?
51. Container restarts and data is lost — why and how to fix?
52. Container is running but application not accessible — what do you check?
53. How do containers communicate securely with each other?
54. How do you pass secrets to containers securely?
55. How do you update a running container without downtime?
56. A container is using 100% CPU — how do you debug?
57. A vulnerability is found in your Docker image — what do you do?
58. How do you integrate Docker into CI/CD pipelines?

## 6. Advanced Docker Questions
59. Difference between Docker Swarm and Kubernetes?
60. What happens if Docker daemon crashes?
61. Explain rootless Docker.
62. How does Docker handle logging?
63. What are distroless images?
64. How do you debug containers in production?
65. Explain OCI (Open Container Initiative).
