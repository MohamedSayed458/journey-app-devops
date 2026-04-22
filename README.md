
# Journey App DevOps Project

This project demonstrates a **production-style end-to-end DevOps pipeline** for deploying a full-stack web application using Docker, Kubernetes (EKS), Helm, and Jenkins CI/CD.

---

## Tech Stack

- **Frontend:** Nginx (static HTML)
- **Backend:** Node.js (Express)
- **Database:** PostgreSQL
- **Containerization:** Docker
- **Orchestration:** Kubernetes (AWS EKS)
- **Deployment:** Helm
- **CI/CD:** Jenkins (Pipeline)
- **Ingress:** NGINX Ingress Controller
- **Cloud:** AWS (EC2 + EKS + LoadBalancer)

---

## Project Structure

```

frontend/ # Static web app (Nginx)  
backend/ # Node.js API  
helm/ # Helm chart for full deployment  
k8s/ # Raw Kubernetes manifests (reference only)  
docker-compose.yml # Local development setup  
Jenkinsfile # CI/CD pipeline definition

```

---

## CI/CD Pipeline (Jenkins)

This project includes a fully automated CI/CD pipeline:

```

GitHub Push  
↓  
Jenkins Pipeline  
↓  
Build Docker Images (frontend & backend)  
↓  
Push Images to Docker Hub  
↓  
Deploy to Kubernetes using Helm  
↓  
Rolling Update on EKS Cluster

````

### Automation

- GitHub Webhook triggers Jenkins automatically on every push
- No manual deployment required
- Zero-downtime rolling updates using Kubernetes

---

## Application Routing

Using NGINX Ingress Controller:

- `/` → Frontend (Nginx)
- `/api` → Backend (Node.js)

---

## Secrets Management

- Kubernetes Secrets are used for:
  - Database credentials
  - Backend environment variables

---

## Deployment

Deploy manually with Helm:

```bash
helm upgrade --install journey-app ./helm/journey-app
````

Or automatically via Jenkins pipeline.

---

## Live Application

The application is exposed using an AWS LoadBalancer:

👉 [http://a12dbdb2a78324118ba89471eb8275c4-e04e8f518750cc31.elb.eu-north-1.amazonaws.com/](http://a12dbdb2a78324118ba89471eb8275c4-e04e8f518750cc31.elb.eu-north-1.amazonaws.com/)

---

## Author

**Mohamed Newish**  
DevOps Engineer | Cloud | Kubernetes | CI/CD