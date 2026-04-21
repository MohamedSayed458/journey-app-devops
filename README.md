# Journey App DevOps Project

This project demonstrates a complete DevOps-ready web application deployment using Kubernetes and Helm.

## Tech Stack

- **Frontend:** Nginx (static HTML)
- **Backend:** Node.js (Express)
- **Database:** PostgreSQL
- **Containerization:** Docker
- **Orchestration:** Kubernetes (EKS)
- **Deployment:** Helm
- **CI/CD (planned):** Jenkins
- **Cloud:** AWS (EC2 + LoadBalancer)

---

## Project Structure

```

frontend/ # Static web app (Nginx)  
backend/ # Node.js API  
helm/ # Helm chart for full deployment  
k8s/ # Raw manifests (reference only)  
docker-compose.yml

````

---

## Features

- 🔹 Full Kubernetes deployment using Helm
- 🔹 Ingress routing:
  - `/` → frontend
  - `/api` → backend
- 🔹 Dockerized frontend and backend
- 🔹 PostgreSQL database running inside the cluster
- 🔹 Kubernetes Secrets used for database configuration
- 🔹 Clean and scalable architecture ready for CI/CD

---

## Deployment

Deploy the application using Helm:

```bash
helm upgrade --install journey-app ./helm/journey-app
````

---

## Access

The application is exposed using an AWS LoadBalancer via NGINX Ingress Controller.

🌐 Live URL:
http://a12dbdb2a78324118ba89471eb8275c4-e04e8f518750cc31.elb.eu-north-1.amazonaws.com/


---

## Author

Mohamed Newish (DevOps / Cloud / Kubernetes)