# 🚀 CI/CD Static Website Deployment (Docker + GitHub Actions + AWS EC2)

A real-world DevOps project that automates the deployment of a **static HTML website** using **Docker + Nginx + GitHub Actions CI/CD + AWS EC2**.

✅ Every time I push code to the `main` branch, the pipeline automatically:
- Builds a Docker image
- Pushes the image to DockerHub
- Deploys the latest image to an AWS EC2 server via SSH

---

## 📌 Live Demo
🌐 **Website URL:** `http://<EC2_PUBLIC_IP>`

---

## 🧰 Tech Stack
- **HTML/CSS** (Static Website)
- **Docker** (Containerization)
- **Nginx** (Web Server)
- **GitHub Actions** (CI/CD Automation)
- **AWS EC2 (Ubuntu)** (Deployment Server)

---

## ✅ Key DevOps Concepts Covered
- Docker image build and push automation
- GitHub Actions workflows
- Secrets management using GitHub Secrets
- SSH-based remote deployment on Linux server
- Container lifecycle management (pull → stop → run)
- Production-style deployment using port **80**

---

## 🏗️ Architecture (Pipeline Flow)

```text
Developer → Git Push (main)
        → GitHub Actions (CI)
        → Docker Image Build
        → Push to DockerHub
        → GitHub Actions (CD)
        → SSH into EC2
        → Pull Latest Image
        → Restart Website Container (Port 80)```

## 📂 Project Structure

```text
devops-static-site/
├── index.html
├── Dockerfile
└── .github/
    └── workflows/
        ├── build-push.yml
        └── deploy.yml
```

---

## 🐳 Docker (Local Setup)

### 1) Build Docker Image
```bash
docker build -t devops-static-site:latest .
```

### 2) Run Container
```bash
docker run -d -p 8080:80 --name website devops-static-site:latest
```

### 3) Access Website
Open in browser:
```text
http://localhost:8080
```

### 4) Stop & Remove Container
```bash
docker rm -f website
```

---

## ☁️ AWS EC2 Setup (Deployment Server)

### ✅ EC2 Requirements
- Ubuntu 22.04 EC2 instance  
- Docker installed and running  
- Security Group inbound rules:
  - **SSH (22)** → My IP
  - **HTTP (80)** → `0.0.0.0/0`

### ✅ Install Docker on EC2
```bash
sudo apt update -y
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo usermod -aG docker ubuntu
newgrp docker
```

---

## 🔐 GitHub Secrets Configuration (Required ✅)

Go to:  
**GitHub Repo → Settings → Secrets and variables → Actions**

Add these repository secrets:

| Secret Name | Description |
|------------|-------------|
| `DOCKER_USERNAME` | Your DockerHub username |
| `DOCKER_PASSWORD` | Your DockerHub password / access token |
| `EC2_HOST` | EC2 Public IPv4 address |
| `EC2_USER` | `ubuntu` |
| `EC2_SSH_KEY` | EC2 private key content (PEM) |

---

## ⚙️ GitHub Actions Workflows

### ✅ Workflow 1: Build & Push Image
📌 `.github/workflows/build-push.yml`

**Trigger:** `push` to `main`

**Jobs:**
- Checkout repository code
- Login to DockerHub
- Build Docker image
- Push image to DockerHub

---

### ✅ Workflow 2: Deploy to EC2
📌 `.github/workflows/deploy.yml`

**Trigger:** after successful build (or direct push based on setup)

**Jobs:**
- SSH into EC2
- Pull latest image from DockerHub
- Stop old container
- Run new container on port **80**

---

## ✅ Deployment Verification (On EC2)

### Check running container
```bash
docker ps
```

### Check website response
```bash
curl -I http://localhost
```

Expected output:
```text
HTTP/1.1 200 OK
Server: nginx
```

---

## 🧪 How to Trigger CI/CD

Make a change in `index.html` and push:

```bash
git add .
git commit -m "Update website content"
git push origin main
```

Then check:
✅ GitHub → **Actions tab** → Workflow should run successfully.

---

## 🧠 What I Learned from This Project
- How CI/CD works in real DevOps environments
- How to containerize a static website using Docker + Nginx
- How to automate deployments using GitHub Actions and SSH
- How to manage and protect credentials using GitHub Secrets
- How to troubleshoot server deployment issues (ports, security group, docker logs)

---

## 📌 Future Improvements
- Add custom domain + HTTPS (Let's Encrypt)
- Add Slack/Email notification for deployments
- Add versioning tags instead of `latest`
- Deploy using Docker Compose for better manageability
- Add monitoring (CloudWatch / Prometheus)

---

## 👨‍💻 Author
**Afzal KH**  
DevOps Learner | AWS | Linux | Docker | CI/CD

⭐ If you like this project, give it a star!

