# 🚀 Production-Ready EKS Deployment with GitHub Actions CI/CD

![AWS](https://img.shields.io/badge/AWS-EKS-orange?style=for-the-badge&logo=amazonaws)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-CI%2FCD-blue?style=for-the-badge&logo=githubactions)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED?style=for-the-badge&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?style=for-the-badge&logo=kubernetes)
![Python](https://img.shields.io/badge/Python-Flask-green?style=for-the-badge&logo=python)

---

## 📌 Project Overview

This project demonstrates a **fully automated CI/CD pipeline** that builds, pushes, and deploys a containerized Python Flask application to **AWS EKS (Elastic Kubernetes Service)** using **GitHub Actions**.

Every time code is pushed to the `main` branch, the pipeline automatically:
- Builds a Docker image
- Pushes it to **AWS ECR (Elastic Container Registry)**
- Deploys the updated image to an **EKS Kubernetes cluster**
- Exposes the app via a **LoadBalancer Service**

> ✅ Zero manual deployments. Zero downtime. Fully automated.

---

## 🏗️ Architecture

```
Developer
    │
    │  git push
    ▼
GitHub Repository
    │
    │  Triggers
    ▼
GitHub Actions Pipeline
    ├── 1. Checkout Code
    ├── 2. Configure AWS Credentials
    ├── 3. Build Docker Image
    ├── 4. Push to AWS ECR
    └── 5. Deploy to AWS EKS
                │
                ▼
        AWS EKS Cluster
                │
        ┌───────┴────────┐
        │                │
   Deployment        LoadBalancer
   (Flask Pods)        Service
        │                │
        └───────┬────────┘
                │
                ▼
         End Users 🌍
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Application** | Python (Flask) |
| **Containerization** | Docker |
| **Container Registry** | AWS ECR |
| **Orchestration** | AWS EKS (Kubernetes) |
| **CI/CD** | GitHub Actions |
| **Cloud Provider** | AWS |
| **K8s Manifests** | Deployment + LoadBalancer Service |

---

## 📁 Project Structure

```
eks-with-git-actions/
├── .github/
│   └── workflows/
│       └── cicd.yml          # GitHub Actions pipeline definition
├── code/
│   └── app.py                # Python Flask application
├── docker/
│   └── Dockerfile            # Docker image build instructions
├── manifests/
│   ├── hello-app-deployment.yaml   # K8s Deployment manifest
│   └── hello-app-service.yaml      # K8s LoadBalancer Service manifest
└── README.md
```

---

## ⚙️ CI/CD Pipeline Flow

### Step 1 — Code Push
Developer pushes code to the `main` branch on GitHub.

### Step 2 — GitHub Actions Triggered
The pipeline defined in `.github/workflows/cicd.yml` is automatically triggered.

### Step 3 — Docker Build
```bash
docker build -t flask-app .
```
The Flask application is packaged into a Docker image.

### Step 4 — Push to AWS ECR
```bash
docker tag flask-app:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/flask-app:$GITHUB_SHA
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/flask-app:$GITHUB_SHA
```
The image is tagged with the Git commit SHA for traceability and pushed to ECR.

### Step 5 — Deploy to EKS
```bash
kubectl apply -f manifests/hello-app-deployment.yaml
kubectl apply -f manifests/hello-app-service.yaml
```
Kubernetes pulls the latest image from ECR and rolls out the update with zero downtime.

---

## 🚀 Setup & Deployment Guide

### Prerequisites
- AWS Account with EKS cluster running
- AWS ECR repository created
- GitHub repository with Actions enabled
- `kubectl` configured for your EKS cluster

### Step 1 — Clone the Repository
```bash
git clone https://github.com/mukeshsaini143/eks-with-git-actions.git
cd eks-with-git-actions
```

### Step 2 — Configure GitHub Secrets
Add the following secrets in your GitHub repository (`Settings → Secrets → Actions`):

| Secret Name | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | Your AWS access key |
| `AWS_SECRET_ACCESS_KEY` | Your AWS secret key |
| `AWS_REGION` | AWS region (e.g., `us-east-1`) |
| `ECR_REPOSITORY` | Your ECR repo name |
| `EKS_CLUSTER_NAME` | Your EKS cluster name |

### Step 3 — Update Manifests
In `manifests/hello-app-deployment.yaml`, the `DOCKER_IMAGE` placeholder is automatically replaced by the pipeline with the actual ECR image URL during deployment.

### Step 4 — Push to Main Branch
```bash
git add .
git commit -m "deploy: trigger CI/CD pipeline"
git push origin main
```
This triggers the full pipeline automatically! ✅

---

## 📦 Kubernetes Manifests Explained

### Deployment (`hello-app-deployment.yaml`)
- Pulls the Docker image from ECR
- Runs the Flask app as pods with auto-scaling replicas
- Uses labels to connect with the Kubernetes Service
- Exposes port `8080` from the container

### Service (`hello-app-service.yaml`)
- Type: `LoadBalancer` — exposes the app to the internet
- Routes external traffic to the Flask pods on port `8080`
- AWS automatically provisions an ELB (Elastic Load Balancer)

---

## ✅ Key Outcomes & Results

- ⚡ **Deployment time reduced** from manual (30+ mins) to fully automated (under 5 mins)
- 🔁 **Every git push** triggers a full build, push, and deploy cycle automatically
- 🐳 **Docker images tagged** with Git commit SHA for full traceability
- 🔒 **AWS credentials** securely stored as GitHub Secrets — never exposed in code
- 📈 **Zero-downtime deployments** using Kubernetes rolling updates

---

## 🔗 References & Learning Resources

- [AWS EKS Documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Build & Push Docker to ECR with GitHub Actions](https://towardsaws.com/build-push-docker-image-to-aws-ecr-using-github-actions-8396888a8f9e)
- [Kubernetes Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

---

## 👨‍💻 Author

**Mukesh K.** — DevOps Engineer with 5+ years of experience in AWS, Kubernetes, Terraform, and CI/CD automation.

🔗 [Upwork Profile](https://www.upwork.com/freelancers/~01f487114810d5b082) | [GitHub](https://github.com/mukeshsaini143)

---

> ⭐ If this project helped you, please give it a star!
