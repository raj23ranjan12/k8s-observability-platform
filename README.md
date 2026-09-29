# 🚀 End-to-End Platform Engineering Project (Docker + Kubernetes + CI/CD + Monitoring)

## 📌 Overview

This project demonstrates a **real-world production-like platform engineering workflow** covering:

- REST API development using Python (Flask)
- Containerization using Docker
- Orchestration using Kubernetes (Minikube / Docker Desktop)
- CI/CD automation using GitHub Actions
- Observability using Prometheus & Grafana
- Failure simulation & auto-healing validation

It simulates how modern cloud-native applications are built, deployed, and monitored in production environments.

---

## 🏗️ Architecture

```
GitHub Repository
        ↓
GitHub Actions (CI/CD Pipeline)
        ↓
Docker Image Build
        ↓
Kubernetes Deployment (Minikube / Docker Desktop)
        ↓
Service Exposure (NodePort)
        ↓
Flask Application (Running Pods)
        ↓
Prometheus (Metrics Collection)
        ↓
Grafana (Visualization & Dashboards)
```

---

## ⚙️ Tech Stack

- Python (Flask)
- Docker
- Kubernetes (Minikube / Docker Desktop Kubernetes)
- GitHub Actions (CI/CD)
- Prometheus (Monitoring)
- Grafana (Dashboards)
- Kubectl CLI

---

# 🧱 Step 1: Application Setup (Flask API)

## 📁 Create Project

```bash
mkdir platform-project
cd platform-project
```

## 📦 Install Dependencies

```bash
pip install flask
```

## 🧾 Create `app.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "🚀 Platform App Running Successfully"

@app.route("/health")
def health():
    return "OK"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

## ▶️ Run Locally

```bash
python app.py
```

### 🔍 What this does:
- `/` → Main application endpoint
- `/health` → Health check endpoint (used in Kubernetes probes / monitoring)

---

# 🐳 Step 2: Dockerization

## 🧾 Create `Dockerfile`

```dockerfile
FROM python:3.9

WORKDIR /app

COPY . .

RUN pip install flask

EXPOSE 5000

CMD ["python", "app.py"]
```

## 🏗️ Build Image

```bash
docker build -t platform-app .
```

## ▶️ Run Container

```bash
docker run -p 5000:5000 platform-app
```

### 🔍 Why Docker?
- Ensures environment consistency
- Portable across dev, test, production

---

# ☸️ Step 3: Kubernetes Deployment

## 🔍 Check Cluster

```bash
kubectl get nodes
```

## 🧾 Create `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: platform-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: platform-app
  template:
    metadata:
      labels:
        app: platform-app
    spec:
      containers:
      - name: platform-app
        image: platform-app
        ports:
        - containerPort: 5000
```

---

## 🧾 Create Service

```bash
kubectl expose deployment platform-app \
  --type=NodePort \
  --port=5000
```

## 🚀 Apply Deployment

```bash
kubectl apply -f deployment.yaml
```

## 🌐 Access Application

```bash
minikube service platform-app
```

### 🔍 Kubernetes Concepts Used:
- Deployment → Ensures desired replicas
- Service → Exposes application externally
- Self-healing → Auto-restarts failed pods

---

# 🔁 Step 4: CI/CD Pipeline (GitHub Actions)

## 🧾 Create Workflow

`.github/workflows/deploy.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Build Docker Image
      run: docker build -t platform-app .

    - name: Run Container Test
      run: docker run -d -p 5000:5000 platform-app
```

### 🔍 What this does:
- On every push → builds Docker image
- Runs container test automatically
- Ensures application stability before deployment

---

# 📊 Step 5: Monitoring Setup (Prometheus + Grafana)

## 📦 Install Helm Repo

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

## 🚀 Install Monitoring Stack

```bash
helm install monitoring prometheus-community/kube-prometheus-stack
```

## 🌐 Access Grafana

```bash
kubectl port-forward service/monitoring-grafana 3000:80
```

Login:
```
Username: admin
Password: prom-operator
```

### 🔍 Monitoring Features:
- Prometheus → Metrics collection
- Grafana → Dashboards & visualization
- Kubernetes cluster health monitoring

---

# 💥 Step 6: Failure Simulation (Important for Interviews)

## 🔥 Kill a Pod

```bash
kubectl delete pod <pod-name>
```

👉 Kubernetes automatically recreates it (self-healing)

---

## 📈 Scale Application

```bash
kubectl scale deployment platform-app --replicas=5
```

---

## ⚡ Load Test (Optional)

```bash
while true; do curl http://localhost:5000; done
```

### 🔍 What this proves:
- Auto-healing capability
- Horizontal scaling
- Resilience under load

---

# 📁 Project Structure
```
platform-project/
│
├── app.py
├── Dockerfile
├── deployment.yaml
├── .github/
│   └── workflows/
│       └── deploy.yml
├── README.md
```

---

# 🧠 Key Learnings

- REST API development using Flask
- Docker containerization best practices
- Kubernetes deployments & services
- CI/CD automation using GitHub Actions
- Observability using Prometheus & Grafana
- Failure simulation & system resilience

---

# 📌 Final Outcome

This project demonstrates a **production-grade DevOps / Platform Engineering workflow** used in real companies:

✔ Build → Containerize → Deploy → Monitor → Automate  
✔ Strong foundation for SRE / Platform Engineer interviews  
✔ Shows end-to-end cloud-native system design understanding  

---

# 👨‍💻 Author

Rajeev Ranjan
SRE / DevOps Engineer  
