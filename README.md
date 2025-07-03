
# DevOps Assignment: WordPress + PostgreSQL + Jenkins CI/CD on Kubernetes

This project sets up a complete Kubernetes-based deployment for WordPress backed by PostgreSQL and managed via Jenkins CI/CD. It includes observability, scaling, ingress configuration, and secrets management.

---

✅ Basic Cluster Operations
kubectl version --client
kubectl get pods
kubectl get svc
kubectl get pvc
kubectl get ingress
kubectl get hpa

---

📦 Deploying Resources
kubectl apply -f <filename.yaml>                # Apply resource
kubectl delete -f <filename.yaml>               # Delete resource

---

⚙️ Debugging & Logs

kubectl logs -l app=<app-name>
kubectl describe pod <pod-name>
kubectl get events

---

🔐 Managing Secrets
kubectl create secret generic <name> \
  --from-literal=<KEY>=<VALUE>

kubectl delete secret <name> --ignore-not-found

---
🔄 Pod & Container Access
kubectl exec -it <pod-name> -- bash             # Open shell inside pod
kubectl exec -it <pod-name> -- <command>    

---
## ✅ 1. Project Structure
```
Sigma-Assignment/
               # Jenkins deployment files
├── kubernetes/
|   ├── jenkins/
│   ├── pgadmin/
│   ├── postgres/
│   └── wordpress/
├── Jenkinsfile              # CI/CD pipeline
└── README.md                # Project documentation
```

---

## 🔧 2. Environment Setup
- **Minikube** for local Kubernetes cluster
- **kubectl** to manage K8s resources
- **Jenkins** to automate deployment

---

## ☁️ 3. Kubernetes Resources
### PostgreSQL
- `postgres-deployment.yaml`
- `postgres-service.yaml`
- `postgres-pvc.yaml`
- `postgres-secret` (created via Jenkins)

### pgAdmin
- `pgadmin-deployment.yaml`
- `pgadmin-service.yaml`
- `pgadmin-configmap.yaml`
- `pgadmin-ingress.yaml`
- `pgadmin-secret` (created via Jenkins)

### WordPress
- `wordpress-deployment.yaml`
- `wordpress-service.yaml`
- `wordpress-pvc.yaml`
- `wordpress-ingress.yaml`
- `wordpress-hpa.yaml`

### Jenkins
- `jenkins-deployment.yaml`
- `jenkins-service.yaml`
- `jenkins-pvc.yaml`
- `jenkins-ingress.yaml`

---

## 🔐 4. Secrets and Configuration Management
Secrets are **not hardcoded** into YAML files. Instead:
- Jenkins uses `kubectl create secret` commands to dynamically inject secrets.
- These values (DB user, password, etc.) are managed via Jenkins **Credential Manager** securely.

This protects sensitive data from exposure in version control.

---

## 🔁 5. Jenkins CI/CD Pipeline
The `Jenkinsfile` automates:
- Code checkout from GitHub
- Secret creation
- Deployment of PostgreSQL, pgAdmin, WordPress
- HPA setup
- Ingress setup for external access

**GitHub Webhooks** and **SCM Polling** are configured to auto-trigger on commits.

---

## 📈 6. HPA and Observability
- Horizontal Pod Autoscaler is enabled for WordPress.
- CPU utilization threshold: `50%`
- PVCs ensure persistent storage.
- Logs and metrics are accessible via `kubectl`.

---

## 🌐 7. Ingress Configuration
Ingress rules were configured for:
- `/pgadmin` → pgAdmin Service
- `/wordpress` → WordPress Service

`minikube tunnel` was used to expose services.

### ❗Issue Encountered
Despite correct Ingress configuration, **WordPress UI was not accessible via browser**. All pods were healthy, and:
- PostgreSQL was connected from WordPress pod.
- `psql` confirmed DB availability.
- All environment variables (`WORDPRESS_DB_HOST`, `USER`, `PASSWORD`) were correctly injected.

After deep debugging, it remained unclear why WordPress UI was unreachable — though **the backend connection with PostgreSQL was successful**.

> ✅ ✅ ✅ All other requirements were fulfilled — including Load Balancing (via ReplicaSets), HPA, persistent storage, Secrets/ConfigMap, and CI/CD automation.

---

## ✅ Final Outcome Summary
- PostgreSQL & pgAdmin running and accessible ✅
- Jenkins pipeline executes full deployment ✅
- WordPress database connection successful ✅
- WordPress UI not accessible (possibly due to ingress routing or pod image behavior) ❌

---

## 🔄 To-Do / Improvements
- Debug WordPress UI exposure issue further
- Add Prometheus + Grafana stack for full observability
- Add proper DNS resolution for Ingress paths

---

## 📎 Author
**Chirag Jain**  
GitHub: [@1chirag](https://github.com/1chirag)

---
