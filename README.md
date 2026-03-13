# DevSecOps CI/CD Pipeline for Netflix Clone Deployment

![DevSecOps Architecture](https://github.com/juhisinha422/DevSecOps-Project-Netflix-Deployment/blob/main/public/assets/Project_Netflix.png)

![DevSecOps Architecture](https://github.com/juhisinha422/DevSecOps-Project-Netflix-Deployment.git/blob/main/public/assets/architecture.png)

## Overview

This project demonstrates a **complete DevSecOps CI/CD pipeline** for deploying a **Netflix Clone application** using modern DevOps and security tools.

The pipeline automates:

- Code Integration
- Static Code Analysis
- Dependency Vulnerability Scanning
- Container Security Scanning
- Docker Image Build & Push
- GitOps Continuous Deployment
- Kubernetes Deployment
- Monitoring & Observability

The goal of this project is to showcase **real-world DevSecOps practices used in production environments.**

---

# Architecture

The pipeline follows this workflow:

Developer → GitHub → Jenkins CI Pipeline → Security Scans → Docker Build → DockerHub → GitOps Update → ArgoCD → Kubernetes → Netflix Application → Monitoring Stack

Pipeline flow:

```
Developer
   │
   ▼
GitHub Repository
   │
   ▼
Jenkins CI Pipeline
   │
   ├ SonarQube (Code Quality Analysis)
   ├ OWASP Dependency Check (Dependency Vulnerabilities)
   ├ Trivy (Filesystem Scan)
   │
   ▼
Docker Build
   │
   ▼
DockerHub
   │
   ▼
Update Kubernetes Manifest
   │
   ▼
GitHub (GitOps Repository)
   │
   ▼
ArgoCD
   │
   ▼
Kubernetes Cluster (K3s)
   │
   ▼
Netflix Clone Application
   │
   ▼
Monitoring Stack
   ├ Prometheus
   ├ Grafana
   ├ Node Exporter
   └ cAdvisor
```

---

# Tech Stack

| Category | Tools |
|--------|------|
| CI/CD | Jenkins |
| Version Control | GitHub |
| Code Quality | SonarQube |
| Dependency Security | OWASP Dependency Check |
| Container Security | Trivy |
| Containerization | Docker |
| Container Registry | DockerHub |
| GitOps Deployment | ArgoCD |
| Container Orchestration | Kubernetes (K3s) |
| Monitoring | Prometheus |
| Visualization | Grafana |
| Metrics Exporters | Node Exporter, cAdvisor |
| Infrastructure | AWS EC2 |

---

# Infrastructure Setup

All tools are installed on an **AWS EC2 Ubuntu instance**.

Installed components:

- Jenkins
- SonarQube
- Docker
- Trivy
- OWASP Dependency Check
- ArgoCD
- Kubernetes (K3s)
- Prometheus
- Grafana
- Node Exporter
- cAdvisor

---

# CI/CD Pipeline

The Jenkins pipeline performs the following stages.

### 1️⃣ Clean Workspace

```
cleanWs()
```

---

### 2️⃣ Checkout Source Code

Jenkins pulls the code from GitHub.

```
git clone https://github.com/juhisinha422/DevSecOps-Project-Netflix-Deployment.git
```

---

### 3️⃣ SonarQube Static Code Analysis

Performs code quality and vulnerability scanning.

Checks include:

- Bugs
- Security vulnerabilities
- Code smells
- Maintainability

Pipeline waits for **Quality Gate validation**.

---

### 4️⃣ Dependency Vulnerability Scan

OWASP Dependency Check scans the project for vulnerable libraries.

```
dependencyCheck --scan ./
```

Detects:

- Known CVEs
- Outdated dependencies

---

### 5️⃣ Filesystem Security Scan

Trivy scans the project source code.

```
trivy fs .
```

Detects:

- Secrets
- Misconfigurations
- Vulnerable packages

---

### 6️⃣ Docker Image Build

Docker image is built using Jenkins.

```
docker build -t netflix .
```

---

### 7️⃣ Push Image to DockerHub

The built Docker image is pushed to DockerHub.

```
docker tag netflix mrsiddu73/netflix:latest
docker push mrsiddu73/netflix:latest
```

---

### 8️⃣ Container Image Security Scan

Trivy scans the built Docker image.

```
trivy image mrsiddu73/netflix:latest
```

---

### 9️⃣ Deploy Container

The application is deployed as a container.

```
docker run -d -p 8081:80 mrsiddu73/netflix:latest
```

---

# Continuous Deployment using ArgoCD

The project follows a **GitOps deployment model**.

Workflow:

1. Jenkins builds and pushes the Docker image.
2. Jenkins updates the Kubernetes manifest.
3. The updated manifest is pushed to GitHub.
4. ArgoCD detects the changes.
5. ArgoCD automatically deploys the updated application to Kubernetes.

---

# Kubernetes Deployment

Application is deployed using Kubernetes manifests.

```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

The application is exposed using a **NodePort service**.

Access the application:

```
http://<EC2_PUBLIC_IP>:<NODEPORT>
```

---

# Monitoring & Observability

Monitoring stack includes:

## Prometheus

Prometheus collects metrics from:

- Node Exporter
- Jenkins
- Kubernetes
- cAdvisor

Metrics include:

- CPU usage
- Memory usage
- Disk usage
- Network traffic
- Container statistics

---

## Grafana

Grafana visualizes the metrics collected by Prometheus.

Dashboards used:

| Dashboard | ID |
|--------|------|
| Node Exporter Full | 1860 |
| Jenkins Monitoring | 9964 |
| Prometheus Overview | 3662 |

---

# Metrics Exporters

## Node Exporter

Monitors system metrics:

- CPU usage
- Memory usage
- Disk usage
- Network traffic

---

## cAdvisor

Monitors container metrics:

- Container CPU usage
- Container memory usage
- Container filesystem
- Container network statistics

---

# Jenkins Email Notifications

Jenkins sends email notifications after pipeline execution.

Example configuration:

```
post {
 always {
  emailext attachLog: true,
   subject: "'${currentBuild.result}'",
   body: "Build Number: ${env.BUILD_NUMBER}",
   to: 'your-email@example.com'
 }
}
```

---

# Live Application

Access the deployed application:

```
http://<EC2_PUBLIC_IP>:<NODEPORT>
```

Example:

```
http://13.xxx.xxx.xxx:30007
```

---

**Access your Application**
   - To Access the app make sure port 30007 is open in your security group and then open a new tab paste your NodeIP:30007, your app should be running.
  
  ![](https://github.com/MrSiddu73/DevSecOps-Project-Netflix-Deployment/blob/main/public/assets/home_page.jpeg)

<div align="center">
  <p align="center">Home Page</p>
</div>


# Future Improvements

Possible enhancements:

- Infrastructure provisioning with Terraform
- Helm charts for Kubernetes deployment
- GitHub Actions CI pipeline
- Slack notifications
- Alertmanager integration
- Horizontal Pod Autoscaling

---

# Author

**Juhi Sinha**

Computer Science & Engineering  
Cloud & DevOps Enthusiast

GitHub  
https://github.com/juhisinha422

---

# License

This project is licensed under the **MIT License**.
