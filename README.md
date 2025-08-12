# etech_helm_module
# 🚀 Helm & Helm Charts – Hands-on DevOps Training Guide

> This guide is designed for **practical learning** — every concept has a **try-it-now** section so you can follow along and see results instantly.

---

## 1️⃣ Install Helm

### Commands
```bash
# Install Helm (Linux/macOS)
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Windows (Chocolatey)
choco install kubernetes-helm

# Verify
helm version
```

✅ **Try It Now:** Run `helm version` — you should see your Helm version and Git commit hash.

---

## 2️⃣ Add a Chart Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx
```

✅ **Try It Now:** Run `helm search repo mysql` to find MySQL charts.

---

## 3️⃣ Deploy an Application (NGINX)

```bash
helm install mynginx bitnami/nginx
helm list
helm status mynginx
```

✅ **Try It Now:** Open your Kubernetes dashboard or run:
```bash
kubectl get svc
```
Find the `EXTERNAL-IP` of `mynginx` and open it in your browser.

---

## 4️⃣ Customize Deployment with `values.yaml`

1. Create a file `custom-values.yaml`:
```yaml
replicaCount: 3
service:
  type: NodePort
  nodePort: 30080
```

2. Upgrade your release:
```bash
helm upgrade mynginx bitnami/nginx -f custom-values.yaml
```

✅ **Try It Now:** Run `kubectl get pods` and confirm **3 replicas** are running.

---

## 5️⃣ Explore a Helm Chart Structure

```bash
helm pull bitnami/nginx --untar
tree nginx
```

✅ **Try It Now:** Open `values.yaml` and change `replicaCount` — then redeploy.

---

## 6️⃣ Create Your Own Helm Chart

```bash
helm create myapp
```

Edit:
- `values.yaml` → Set your app image
- `templates/deployment.yaml` → Customize replicas, env vars

Render locally without installing:
```bash
helm template myapp
```

✅ **Try It Now:** Deploy your chart:
```bash
helm install myapp ./myapp
```

---

## 7️⃣ Helm in CI/CD (GitHub Actions Example)

Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy with Helm
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: azure/setup-helm@v3
      - name: Deploy App
        run: |
          helm upgrade --install myapp ./mychart  --namespace prod --create-namespace
```

✅ **Try It Now:** Push changes to `main` and watch GitHub Actions deploy.

---

## 8️⃣ Advanced Helm

### Dependencies
```bash
# Add dependency in Chart.yaml
dependencies:
  - name: redis
    version: 17.8.3
    repository: https://charts.bitnami.com/bitnami

# Install dependencies
helm dependency update
```

✅ **Try It Now:** Add Redis as a dependency to your app chart and redeploy.

---

## 🛠 Student Project
**Deploy a Scalable Voting App on Kubernetes with Helm**
- Create charts for:
  - **frontend** (React)
  - **backend** (Python/Flask)
  - **database** (PostgreSQL)
- Use different `values.yaml` for `dev` and `prod`
- Deploy via GitHub Actions

---

## 📚 Resources
- [Helm Official Docs](https://helm.sh/docs/)
- [Artifact Hub](https://artifacthub.io/)

---
💡 **Tip for Trainers:** Pair this README with a `/labs` folder containing:
```
/labs
  /nginx-custom
  /myapp-chart
  /voting-app
```
Each folder should have its own `README.md` and `values.yaml` so students can clone and run immediately.
