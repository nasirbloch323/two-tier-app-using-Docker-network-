# 🐳 Two-Tier Flask + MySQL App
### Deployed using Docker Networks on AWS EC2

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat&logo=flask&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)

> A Two-Tier web application with Flask backend and MySQL database — connected using Docker Bridge Network and deployed on AWS EC2.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────┐
│         AWS EC2 Instance            │
│                                     │
│  ┌──────────────┐  flask-net  ┌─────────────┐  │
│  │  Flask App   │ ──────────► │    MySQL    │  │
│  │  Port: 5000  │             │  Port: 3306 │  │
│  └──────────────┘             └─────────────┘  │
│                                     │
└─────────────────────────────────────┘
```

---

## 🌐 Live Demo
```
http://YOUR_EC2_IP:5000
```

---

## 🛠️ Tech Stack
| Technology | Usage |
|-----------|-------|
| Python/Flask | Backend |
| MySQL 5.7 | Database |
| Docker | Containerization |
| Docker Network | Container Communication |
| AWS EC2 | Cloud Deployment |

---

## 🚀 Step-by-Step Deployment

### Step 1: Install Dependencies
```bash
# System update karo
sudo apt update

# Docker aur Git install karo
sudo apt install -y docker.io git

# Docker permission do
sudo usermod -aG docker ubuntu
newgrp docker

# Check karo
docker --version
```

---

### Step 2: Project Clone Karo
```bash
# GitHub se clone karo
https://github.com/nasirbloch323/two-tier-app-using-Docker-network-.git
# Folder mein jao
cd two-tier-flask-app

# Files dekho
ls
```

---

### Step 3: Docker Network Banao
```bash
# Private network banao
# Dono containers is network se communicate karenge
docker network create flask-net

# Network verify karo
docker network ls
```

---

### Step 4: Dockerfile Banao
```bash
# Dockerfile create karo
nano Dockerfile
```

```dockerfile
# Base image
FROM python:3.10-slim

# Working directory set karo
WORKDIR /app

# Sari files copy karo
COPY . .

# Dependencies install karo
RUN pip install -r requirements.txt

# Port open karo
EXPOSE 5000

# App start karo
CMD ["python", "app.py"]
```

---

### Step 5: Flask Image Build Karo
```bash
# Docker image build karo
docker build -t flask-app .

# Images verify karo
docker images
```

---

### Step 6: MySQL Container Chalao
```bash
# MySQL container start karo
# flask-net network se connect karo
docker run -d \
--name mysql \
--network flask-net \
-e MYSQL_DATABASE=mydb \
-e MYSQL_ROOT_PASSWORD=admin \
-p 3306:3306 \
mysql:5.7

# Check karo
docker ps
```

---

### Step 7: Flask App Container Chalao
```bash
# Flask container start karo
# MySQL se connect karo environment variables se
docker run -d \
--name flask-app \
--network flask-net \
-e MYSQL_HOST=mysql \
-e MYSQL_USER=root \
-e MYSQL_PASSWORD=admin \
-e MYSQL_DB=mydb \
-p 5000:5000 \
flask-app

# Check karo
docker ps
```

---

### Step 8: Connection Verify Karo
```bash
# Network inspect karo
docker network inspect flask-net

# Flask container mein ghuso
docker exec -it flask-app /bin/sh

# MySQL ping karo
ping mysql

# Bahar aao
exit

# Logs dekho
docker logs flask-app
```

---

### Step 9: AWS Security Group
```
EC2 → Security Groups → Inbound Rules:

Port 5000 → Flask App  → 0.0.0.0/0
Port 3306 → MySQL      → 0.0.0.0/0
Port 22   → SSH        → Your IP
```

---

### Step 10: Browser Mein Kholo
```bash
# EC2 IP nikalo
curl ifconfig.me

# Browser mein:
http://YOUR_EC2_IP:5000
```

---

## ✅ Verify Sab Sahi Hai
```bash
# Containers dekho
docker ps

# Expected Output:
# flask-app → Port 5000 ✅
# mysql     → Port 3306 ✅

# Network dekho
docker network inspect flask-net
# flask-app aur mysql dono network mein ✅
```

---

## 📚 Key Concepts Learned
```
✅ Docker Network = Private road for containers
✅ Bridge Network = Default secure network
✅ Container Communication by Name
✅ Environment Variables for Config
✅ Two-Tier Architecture
✅ AWS EC2 Deployment
```

---

## 🔧 Useful Commands
```bash
# Containers stop karo
docker stop flask-app mysql

# Containers delete karo
docker rm flask-app mysql

# Network delete karo
docker network rm flask-net

# Images delete karo
docker rmi flask-app mysql:5.7
```

---

*Made with ❤️ by Nasir Baloch — DevOps Journey 🚀*
*github.com/nasirbloch323*
