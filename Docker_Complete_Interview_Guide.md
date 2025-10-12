
# 🐳 Docker Complete Interview Guide (Basic + Advanced)

This document combines **Docker fundamentals** and **advanced concepts** with real-time examples, Jenkins pipeline integration, and useful commands for quick reference.

---

## 🧩 1. What is Docker and Why It’s Used in DevOps

**Docker** is a containerization platform that packages an application and its dependencies into a lightweight, portable unit called a container.

In DevOps, it ensures consistent deployments across different environments (dev, test, prod).

**Example (Project):**
> Used Docker to package a Java web application (WAR) with Tomcat and JDK dependencies for deployment on GCP VM via Jenkins.

**Commands:**
```bash
docker build -t myapp:1.0 .
docker run -d -p 8080:8080 myapp:1.0
```

---

## 🧩 2. Difference Between Container and Virtual Machine

| Feature | Container | Virtual Machine |
|----------|------------|----------------|
| OS | Shares Host OS kernel | Has its own OS |
| Size | Lightweight (MBs) | Heavy (GBs) |
| Startup | Seconds | Minutes |
| Isolation | Process-level | Hardware-level |
| Performance | Faster | Slower |

---

## 🧩 3. Dockerfile and Key Instructions

A **Dockerfile** is a script with commands to automate image creation.

| Instruction | Description | Example |
|--------------|--------------|----------|
| `FROM` | Base image | `FROM openjdk:8-jre-alpine` |
| `COPY` | Copy files into image | `COPY target/app.war /usr/local/tomcat/webapps/` |
| `RUN` | Run command during build | `RUN apt-get update` |
| `CMD` | Default command to run | `CMD ["catalina.sh", "run"]` |
| `EXPOSE` | Opens port | `EXPOSE 8080` |
| `ENTRYPOINT` | Main container process | `ENTRYPOINT ["java", "-jar", "app.jar"]` |

**Example Dockerfile:**
```dockerfile
FROM tomcat:9-jdk8
COPY target/NETFLIX-1.2.2.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD ["catalina.sh", "run"]
```

---

## 🧩 4. Jenkins Integration – Build and Push Docker Image

Jenkins pipeline automates Docker image build and push to Docker Hub.

**Example Jenkinsfile:**
```groovy
pipeline {
  agent any
  stages {
    stage('Build Image') {
      steps {
        sh 'docker build -t myapp:${BUILD_NUMBER} .'
      }
    }
    stage('Push Image') {
      steps {
        withCredentials([string(credentialsId: 'dockerhub_token', variable: 'DOCKER_PASS')]) {
          sh '''
            echo $DOCKER_PASS | docker login -u saravana2002 --password-stdin
            docker push saravana2002/myapp:${BUILD_NUMBER}
          '''
        }
      }
    }
  }
}
```

**Commands:**
```bash
docker build -t saravana2002/adq-windows-img:latest .
docker push saravana2002/adq-windows-img:latest
```

---

## 🧩 5. Persisting Data – Volumes vs Bind Mounts

Containers are ephemeral. Use **Volumes** or **Bind Mounts** to persist data.

| Type | Description | Example |
|-------|--------------|----------|
| **Volume** | Managed by Docker | `docker run -v mydata:/app/data myapp` |
| **Bind Mount** | Mounts host path | `docker run -v /opt/data:/app/data myapp` |

---

## 🧩 6. Sharing Images (Docker Hub, ECR, GCR)

**Commands:**
```bash
docker tag myapp:1.0 saravana2002/myapp:1.0
docker push saravana2002/myapp:1.0
docker pull saravana2002/myapp:1.0
```

**Registries:**
- Docker Hub (Public/Private)
- AWS ECR (for AWS)
- GCP Artifact Registry / GCR
- Nexus Repository (internal use)

---

## 🧩 7. Docker Networking

**Network Types:**
| Type | Description | Example |
|-------|-------------|----------|
| Bridge | Default network | `docker network create mynet` |
| Host | Uses host network | `docker run --network host nginx` |
| None | No network | Isolated containers |
| Overlay | Multi-host (Swarm/K8s) | Used in orchestration |
| Macvlan | Assigns MAC address | Appears as physical device |

**Commands:**
```bash
docker network ls
docker network inspect mynet
docker run -d --name web --network mynet nginx
```

---

## 🧩 8. Container Troubleshooting

**Useful Commands:**
```bash
docker ps -a
docker logs <container>
docker inspect <container>
docker exec -it <container> /bin/bash
docker top <container>
```

**Example Debug Flow:**
```bash
docker logs myapp
docker inspect myapp | grep Error
```

---

## 🧩 9. Multi-Stage Docker Builds

Used to minimize image size by separating build and runtime stages.

**Example:**
```dockerfile
# Stage 1: Build
FROM maven:3.8.5-openjdk-11 AS build
WORKDIR /app
COPY . .
RUN mvn clean package

# Stage 2: Run
FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=build /app/target/myapp.war /app/myapp.war
CMD ["java", "-jar", "myapp.war"]
```

---

## 🧩 10. Docker Compose – Multi-Container Management

**Example `docker-compose.yml`:**
```yaml
version: '3'
services:
  app:
    image: myapp:1.0
    ports:
      - "8080:8080"
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
```

**Commands:**
```bash
docker-compose up -d
docker-compose ps
docker-compose down
```

---

## 🧩 11. Handling Environment Variables and Secrets

**CLI Example:**
```bash
docker run -e DB_USER=admin -e DB_PASS=secret myapp
```

**From `.env` File:**
```bash
docker run --env-file .env myapp
```

**Compose Example:**
```yaml
environment:
  - DB_USER=admin
  - DB_PASS=${DB_PASS}
```

**Best Practice:**  
Use external secret managers like HashiCorp Vault, AWS Secrets Manager, or Jenkins credentials.

---

## 🧩 12. Cleanup and Resource Management

**Commands:**
```bash
docker system df                # Check disk usage
docker system prune -af         # Remove all unused resources
docker image prune -a           # Delete unused images
docker volume prune             # Delete unused volumes
docker ps -a | grep Exited      # List stopped containers
```

---

## 🧩 13. Docker Security Best Practices

- Use lightweight base images (e.g., Alpine)
- Scan images using Trivy or `docker scan`
- Use non-root users inside containers
- Keep Docker daemon updated
- Avoid storing credentials inside Dockerfile

**Example:**
```bash
docker scan saravana2002/myapp:latest
```

---

## 🧩 14. Jenkins + Docker + Kubernetes Integration

**Workflow:**
1. Jenkins builds Docker image.  
2. Pushes image to registry (Docker Hub/ECR/GCR).  
3. Kubernetes pulls image and deploys pods.

**Pipeline Example:**
```groovy
pipeline {
  agent any
  stages {
    stage('Build & Push') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub_token') {
            def app = docker.build("saravana2002/myapp:${BUILD_NUMBER}")
            app.push()
          }
        }
      }
    }
  }
}
```

---

## 🧠 Quick Revision Summary

| Concept | Summary |
|----------|----------|
| Docker | Containerization platform for portable deployments |
| Dockerfile | Script to build images |
| Volumes | Persistent storage managed by Docker |
| Networks | Connect containers & external systems |
| Multi-stage Build | Reduces image size |
| Compose | Runs multi-container apps |
| Secrets | Managed externally or via env vars |
| Cleanup | `docker system prune -af` |
| Security | Scan, non-root user, minimal base |
| Jenkins Integration | Build → Push → Deploy |

---

**Prepared by:** Saravana L  
*(DevOps Engineer | Docker | Jenkins | GCP | EKS | Terraform | Ansible)*
