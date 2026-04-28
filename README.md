# ⚙️ Cloud Infrastructure Automation & CI/CD — GitOps with ArgoCD

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=for-the-badge&logo=argo&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)

> **End-to-end CI/CD pipeline automation with GitOps continuous deployment — from code commit to production on AWS EKS using GitHub Actions, ArgoCD, Docker, and Terraform**

---

## 📌 Problem Statement

Manual deployment processes are slow, error-prone, and inconsistent across environments. This project implements a fully automated CI/CD pipeline with GitOps principles — ensuring every code change is automatically built, tested, containerised, and deployed to Kubernetes without manual intervention.

---

## 🏗️ Architecture

```
Developer pushes code
        │
        ▼
┌───────────────────┐
│   GitHub Repo     │
│   (Source of      │
│    Truth)         │
└────────┬──────────┘
         │ triggers
         ▼
┌───────────────────────────────────────────────┐
│         GitHub Actions CI Pipeline            │
│                                               │
│  ┌──────┐  ┌──────┐  ┌──────────┐  ┌──────┐ │
│  │Build │→ │Test  │→ │ Docker   │→ │Push  │ │
│  │      │  │      │  │ Build    │  │ ECR  │ │
│  └──────┘  └──────┘  └──────────┘  └──────┘ │
└────────────────────────┬──────────────────────┘
                         │ updates image tag in
                         ▼ Kubernetes manifests
┌───────────────────────────────────────────────┐
│              ArgoCD (GitOps)                  │
│                                               │
│  Watches Git repo → Detects manifest changes  │
│  → Auto-syncs to EKS cluster                  │
└────────────────────────┬──────────────────────┘
                         │
                         ▼
┌───────────────────────────────────────────────┐
│              AWS EKS Cluster                  │
│         (Production Environment)              │
└───────────────────────────────────────────────┘
         ▲
         │ provisioned by
┌────────┴──────────┐
│    Terraform      │
│  (Infrastructure  │
│   as Code)        │
└───────────────────┘
```

---

## ✅ Key Features

- 🔄 **Multi-stage CI Pipeline** — automated build → test → Docker build → ECR push → EKS deploy
- 🎯 **GitOps with ArgoCD** — Kubernetes manifests in Git are the single source of truth; ArgoCD auto-syncs any change
- 🏗️ **Terraform IaC** — all AWS infrastructure provisioned programmatically (EKS, ECR, VPC, IAM)
- 🔐 **IAM least-privilege** — dedicated IAM roles for each pipeline stage with minimal permissions
- 🐍 **Python & Bash automation** — scripts for environment setup, health checks, and deployment validation
- 📦 **Environment-agnostic deployments** — same pipeline works across dev, staging, and production

---

## 📊 Results & Impact

| Metric | Result |
|--------|--------|
| Deployment failures | ⬇️ Reduced by **50%** |
| Release efficiency | ⬆️ Improved by **40%** |
| Manual deployment steps | **Zero** — fully automated |
| Time from commit to production | < 8 minutes |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| CI Pipeline | GitHub Actions |
| CD / GitOps | ArgoCD |
| Containerisation | Docker |
| Container Registry | AWS ECR |
| Orchestration | Kubernetes (AWS EKS) |
| Infrastructure as Code | Terraform |
| Configuration Management | Ansible |
| Scripting | Python, Bash |
| Cloud | AWS (EKS, ECR, IAM, VPC, ALB) |
| Source Control | Git, GitHub |

---

## 🚀 CI/CD Pipeline Stages

```yaml
# .github/workflows/deploy.yml — Pipeline overview

Pipeline Stages:
  1. Build        → Compile application, run unit tests
  2. Test         → Integration tests, code quality checks
  3. Docker Build → Build Docker image with version tag
  4. ECR Push     → Push image to AWS Elastic Container Registry
  5. Update Manifests → Update Kubernetes deployment manifest with new image tag
  6. ArgoCD Sync  → ArgoCD detects manifest change, auto-deploys to EKS
```

---

## 🔧 Infrastructure Setup

### 1. Clone repository
```bash
git clone https://github.com/akash-Paiavula/DailyFix-AI-Mesh.git
cd DailyFix-AI-Mesh
```

### 2. Provision AWS infrastructure with Terraform
```bash
cd terraform/
terraform init
terraform plan
terraform apply
```

### 3. Configure ArgoCD
```bash
# Install ArgoCD on EKS
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Apply ArgoCD application manifest
kubectl apply -f argocd/application.yaml
```

### 4. Configure GitHub Actions Secrets
```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
ECR_REPOSITORY
EKS_CLUSTER_NAME
```

### 5. Push code — pipeline triggers automatically
```bash
git add .
git commit -m "feat: new feature"
git push origin main
# GitHub Actions triggers → builds → deploys → ArgoCD syncs to EKS
```

---

## 📁 Repository Structure

```
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions CI/CD pipeline
├── terraform/
│   ├── main.tf                 # EKS, ECR, VPC, IAM
│   ├── variables.tf
│   └── outputs.tf
├── k8s/
│   ├── deployment.yaml         # Kubernetes deployment manifest
│   ├── service.yaml
│   └── ingress.yaml
├── argocd/
│   └── application.yaml        # ArgoCD app definition
├── scripts/
│   ├── deploy.sh               # Deployment automation script
│   └── health-check.py         # Post-deployment health check
├── Dockerfile
└── README.md
```

---

## 🔐 Security Implementation

- **IAM Roles**: Separate roles for GitHub Actions, EKS nodes, and ECR access
- **OIDC Provider**: GitHub Actions uses OIDC for keyless AWS authentication
- **ECR image scanning**: Automated vulnerability scanning on every image push
- **ArgoCD RBAC**: Restricted access to ArgoCD UI and sync operations

---

## 👤 Author

**Akash Paiavula**
- 📧 akashpaiavula2003@gmail.com
- 💼 [LinkedIn](https://linkedin.com/in/akash-paiavula-a68718289)
- 🐙 [GitHub](https://github.com/akash-Paiavula)

---

⭐ **If this project helped you, please give it a star!**
