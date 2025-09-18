```sh
kubectl create namespace argocd
```

#### Install ArgoCD

```sh
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Check the status

```sh
kubectl get pods -n argocd
```

The command

```sh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```

**Explanation:**

- Changes the Argo CD server service (`argocd-server`) to type `NodePort` in the `argocd` namespace.
- This exposes the Argo CD UI on a port of your Minikube node, making it accessible from your browser.

**Usage:**  
Run this after installing Argo CD to access its web UI via Minikube’s IP and the assigned node port.

The command you provided retrieves the initial admin password for ArgoCD. Here’s how it works:

```sh
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
```

**Explanation:**

- This fetches the YAML for the `argocd-initial-admin-secret` in the `argocd` namespace.
- The password is stored in the `data.password` field (base64 encoded).

**To decode the password:**

```sh
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

Use this password with username `admin` to log in to the ArgoCD web UI.

```
$ kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
apiVersion: v1
data:
  password: N010eFpRMWRaM0xLT0JkUw==
kind: Secret
metadata:
  creationTimestamp: "2025-09-18T07:11:42Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "191758"
  uid: 8f9337a2-0c30-4df0-8918-46c99ab8a46c
type: Opaque
```

Convert to base64

```sh
echo -n "N010eFpRMWRaM0xLT0JkUw==" | base64
```

7MtxZQ1dZ3LKOBdS

### 1. Build the Docker image inside Minikube

First, set your Docker environment to use Minikube’s Docker daemon:

```sh
eval $(minikube docker-env)
```

Then build the image:

```sh
docker build -t darryl1975/cicd-demo-app:v1 .
```

### 2. Deploy to Minikube using Kubernetes manifests

Apply the deployment and service defined in deployment.yaml:

```sh
kubectl apply -f k8s/deployment.yaml
```

### 3. Expose the application

The service in deployment.yaml exposes port 8080. To access it from your browser, run:

```sh
minikube service cicd-demo-app
```

This will open the service endpoint in your browser.

---

**Summary of files involved:**

- Docker image built from dockerfile
- Kubernetes manifests in deployment.yaml

**Endpoints:**

- Health: `/`
- Version: `/version`
- Nations: `/nations`
- Currencies: `/currencies`

Certainly! Here’s a breakdown of what your README.md is doing, step by step:

---

### 1. **Create Namespace for ArgoCD**

```sh
kubectl create namespace argocd
```

Creates a separate Kubernetes namespace called `argocd` to keep ArgoCD resources organized.

---

### 2. **Install ArgoCD**

```sh
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Downloads and applies the official ArgoCD installation manifests to the `argocd` namespace, installing ArgoCD components.

---

### 3. **Check ArgoCD Pod Status**

```sh
kubectl get pods -n argocd
```

Lists all pods in the `argocd` namespace so you can see if ArgoCD is running.

---

### 4. **Expose ArgoCD Web UI**

```sh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
```

Changes the ArgoCD server service type to `NodePort`, making the web UI accessible from outside the cluster (e.g., your browser).

---

### 5. **Get Initial Admin Password**

```sh
kubectl get secret argocd-initial-admin-secret -n argocd -o yaml
```

Fetches the secret containing the initial admin password for ArgoCD.

**Decode the password:**

```sh
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 --decode
```

Decodes the password so you can log in to the ArgoCD web UI.

---

### 6. **Build Docker Image in Minikube**

```sh
eval $(minikube docker-env)
docker build -t darryl1975/cicd-demo-app:v1 .
```

Configures your shell to use Minikube’s Docker daemon, then builds your app’s Docker image inside Minikube.

---

### 7. **Deploy Application to Minikube**

```sh
kubectl apply -f k8s/deployment.yaml
```

Deploys your application using Kubernetes manifests.

---

### 8. **Expose Application Service**

```sh
minikube service cicd-demo-app
```

Opens your app’s service endpoint in your browser.

---

### **Summary**

- You install and expose ArgoCD for GitOps-based deployments.
- You build and deploy your app inside Minikube.
- You access both ArgoCD and your app via browser.

Let me know if you want more details about any step!
