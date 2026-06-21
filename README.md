<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:4F46E5,100:06B6D4&height=220&section=header&text=YelpCamp%20on%20EKS&fontSize=58&fontColor=ffffff&fontAlignY=35&desc=Enterprise%20DevSecOps%3A%20Terraform%20%E2%86%92%20Jenkins%20%E2%86%92%20ArgoCD%20%E2%86%92%20EKS&descSize=18&descAlignY=55&animation=fadeIn" width="100%"/>

<br/>

<a href="#-overview">Overview</a> •
<a href="#-pipeline-flow">Pipeline</a> •
<a href="#-security-gates">Security</a> •
<a href="#-infrastructure">Infra</a> •
<a href="#-quick-start">Quick Start</a> •
<a href="#-contact">Contact</a>

<br/>

![Build](https://img.shields.io/badge/build-passing-success?style=for-the-badge&logo=jenkins&logoColor=white)
![Quality Gate](https://img.shields.io/badge/sonarqube-passed-brightgreen?style=for-the-badge&logo=sonarqube&logoColor=white)
![Security](https://img.shields.io/badge/trivy-scanned-success?style=for-the-badge&logo=aqua&logoColor=white)
![GitOps](https://img.shields.io/badge/argocd-synced-orange?style=for-the-badge&logo=argo&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)

![GitHub last commit](https://img.shields.io/github/last-commit/jyothisai0336/YelpCamp?style=flat-square&color=4F46E5)
![GitHub repo size](https://img.shields.io/github/repo-size/jyothisai0336/YelpCamp?style=flat-square&color=4F46E5)
![GitHub stars](https://img.shields.io/github/stars/jyothisai0336/YelpCamp?style=flat-square&color=4F46E5)

</div>

<br/>

## 📖 Overview

**YelpCamp** is deployed end-to-end through a fully automated **enterprise DevSecOps pipeline** — from infrastructure provisioning to production traffic. Every commit triggers a pipeline that provisions infrastructure with **Terraform**, runs static analysis and dependency scanning, builds and scans the container image, then hands off to **ArgoCD** for GitOps-driven deployment onto **AWS EKS**, fronted by an **AWS Load Balancer**.

This isn't a manual `kubectl apply`. Infrastructure is code, deployment is declarative, and the pipeline physically cannot promote an image that fails its quality or security checks.

> 💡 **The core idea:** infrastructure, code quality, and container security are all gated *before* Kubernetes ever sees the image — and the cluster state is driven by Git, not by hand.

<br/>

## 🔹 What Was Built

| Layer | Tooling | Purpose |
|---|---|---|
| Infrastructure as Code | **Terraform** | Provisions the AWS EKS cluster and supporting infra |
| Source Control | **Git / GitHub** | Single source of truth, triggers the pipeline |
| CI Orchestration | **Jenkins** (Declarative Pipeline) | Drives build, scan, and release stages |
| Code Quality | **SonarQube** | Static analysis + enforced Quality Gate |
| Dependency Security | **OWASP Dependency-Check** | Software composition analysis (SCA) for known CVEs in dependencies |
| Image Build | **Docker** | Builds the application container |
| Image Security | **Trivy** | Scans the built image for vulnerabilities before push |
| Registry | **DockerHub** | Stores the scanned, approved image |
| GitOps | **ArgoCD** | Syncs Kubernetes manifests from Git to the cluster |
| Orchestration | **AWS EKS** | Runs the application at the cluster level |
| Exposure | **AWS Load Balancer** | Routes external traffic into the cluster |

<br/>

## 🔹 Pipeline Flow

```
Developer Commit
       │
       ▼
     GitHub
       │
       ▼
    Jenkins  ──────────────┐
       │                   │
       ▼                   │
  SonarQube Scan            │
       │                    │
       ▼                    │   CI: Code Quality &
 OWASP Dependency Check      │   Security Gates
       │                     │
       ▼                     │
  Quality Gate Validation    │
       │              (pipeline halts on failure)
       ▼                   │
   Docker Build            │
       │                   │
       ▼                   │
    Trivy Scan ────────────┘
       │
       ▼
  Push to DockerHub
       │
       ▼
 Update K8s Manifests (Git)
       │
       ▼
      ArgoCD  ── detects drift, syncs automatically
       │
       ▼
    AWS EKS
       │
       ▼
  AWS Load Balancer
       │
       ▼
  YelpCamp — live
```

<br/>

## 🔹 Security Gates

| Stage | Tool | What it blocks |
|---|---|---|
| Static Analysis | SonarQube | Code smells, bugs, security hotspots — pipeline halts if the Quality Gate fails |
| Dependency Scan | OWASP Dependency-Check | Known-vulnerable libraries/packages pulled in by the app |
| Image Scan | Trivy | Container images with critical/high CVEs — scanned *before* the push stage |

No image reaches DockerHub, and no manifest reaches ArgoCD, unless it clears every gate above.

<br/>

## 🔹 Infrastructure

- **Provisioning:** Terraform defines and provisions the AWS EKS cluster and its supporting resources
- **Cluster:** AWS EKS (managed Kubernetes control plane)
- **CI runner:** Dedicated Jenkins instance driving the pipeline
- **GitOps controller:** ArgoCD watches the manifests repo and reconciles cluster state automatically
- **Ingress:** AWS Load Balancer exposes the application externally

<br/>

## 🔹 Quick Start

<details>
<summary><b>Clone and explore</b></summary>

```bash
git clone https://github.com/jyothisai0336/YelpCamp.git
cd YelpCamp
```

</details>

<details>
<summary><b>Provision infrastructure</b></summary>

```bash
cd terraform/
terraform init
terraform plan
terraform apply
```

</details>

<details>
<summary><b>Pipeline trigger</b></summary>

Pushing to the main branch triggers the Jenkins Declarative Pipeline automatically, which runs the full flow above and updates the manifests repo that ArgoCD watches.

</details>

<br/>

## 🔹 Notes

- This is a personal portfolio / learning deployment, not a production client system.
- Pipeline stage names and flow reflect the actual Jenkinsfile in this repo.

<br/>

## 📬 Contact

<div align="center">

**Jyothisai Mekala**
DevOps / DevSecOps Engineer

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:mekalajyothisai3@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](www.linkedin.com/in/jyothisai-mekala-6852a822a)

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:4F46E5,100:06B6D4&height=100&section=footer" width="100%"/>

</div>
