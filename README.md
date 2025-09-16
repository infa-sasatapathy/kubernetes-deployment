# Kubernetes Deployment Strategies: Canary & Blue-Green

This repository demonstrates two popular **Kubernetes Deployment Strategies**:

1. **Canary Deployment**  
2. **Blue-Green Deployment**

Both strategies are widely used in production to safely roll out new versions of applications with minimal downtime and risk.

---

## 📂 Files in this Repo

- `canary-deployment.yaml` → Canary Deployment manifest  
- `blue-green-deployment.yaml` → Blue-Green Deployment manifest  
- `service.yaml` → Kubernetes Service definition (shared between strategies)  

---

## 🚀 Canary Deployment

### 🔎 What is Canary Deployment?
Canary deployment means rolling out a new version of an application to a **small subset of users** (a "canary" group) before deploying it cluster-wide.  
If the canary version works well, you gradually increase traffic until 100% runs on the new version.  
If it fails, you roll back without impacting most users.

---

### 📝 How it’s Implemented Here
- **Two Deployments** are created:
  - `myapp` → Stable (v1) with fewer replicas.  
  - `canary-myapp` → Canary (v2) with more replicas.  
- Both Deployments have the same `app: myapp` label (so they are picked up by the same Service).  
- The Service load balances requests across both sets of Pods.  

**Example Traffic Split:**  
- Stable: 2 replicas → ~20% traffic  
- Canary: 8 replicas → ~80% traffic  

Traffic is distributed **based on Pod count**.

---

### ✅ How to Test
1. Apply the manifests:
   ```bash
   kubectl apply -f service.yaml
   kubectl apply -f deployment.yaml       # v1
   kubectl apply -f canary-deployment.yaml # v2
