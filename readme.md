# Kubernetes NFS-backed HTTP Server

##  Setup

### 1. Start Kubernetes Cluster

Using Minikube:

```bash
minikube start
```

### 2. Install NFS Server + Provisioner

```bash
helm repo add kvaps https://kvaps.github.io/charts
helm repo update

helm install nfs-server kvaps/nfs-server-provisioner \
  --set persistence.enabled=true \
  --set persistence.size=1Gi \
  --set storageClass.name=nfs \
  --namespace kube-system
```

### 3. Create PersistentVolumeClaim

```bash
kubectl apply -f pvc.yaml
```

### 4. Deploy nginx Server

```bash
kubectl apply -f deployment.yaml
```

### 5. Create NodePort Service

```bash
kubectl apply -f service.yaml
```

### 6. Create Job to Copy Sample Content

```bash
kubectl apply -f job.yaml
```

### 7. Start server

```bash
minikube service nginx-service --url
```