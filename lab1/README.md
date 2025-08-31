
# Kubernetes Lab 1 – Sidecar Pattern with NFS and Shared Volumes

## 📌 Overview

This lab demonstrates how to deploy a Kubernetes **Pod with multiple containers** using the **sidecar pattern**.
It also shows how to use **shared volumes (`emptyDir`)** for inter-container communication and **NFS volumes** for persistent storage.

The Pod includes:

* **nginx container** → serves static content on port `80`.
* **sidecar container (busybox)** → periodically generates an `index.html` file inside a shared volume.
* **Volumes**:

  * `emptyDir` → allows both containers to share files.
  * `nfs` → mounts an external NFS server directory.

---

## 🖥️ Prerequisites

* A running Kubernetes cluster (kind, minikube, k3d, or any other).
* `kubectl` installed and configured.
* Access to a Linux machine/VM to act as an **NFS server**.

---

## ⚙️ Setting Up the NFS Server

### 1. Install NFS packages

On the server node (replace with your distro):

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install -y nfs-kernel-server

# CentOS/RHEL
sudo yum install -y nfs-utils
```

---

### 2. Create the shared directory

```bash
sudo mkdir -p /mnt/shared
sudo chown nobody:nogroup /mnt/shared
sudo chmod 777 /mnt/shared
```

---

### 3. Configure NFS exports

Edit the exports file:

```bash
sudo nano /etc/exports
```

Add this line (adjust subnet to match your cluster network):

```
/mnt/shared 192.168.100.0/24(rw,sync,no_subtree_check,no_root_squash)
```

---

### 4. Apply export configuration

```bash
sudo exportfs -rav
```

---

### 5. Start and enable the NFS service

```bash
sudo systemctl enable nfs-server
sudo systemctl restart nfs-server
```


---

## 🚀 Deploy the Pod

1. Apply the manifest:

```bash
kubectl apply -f pod.yaml
```

2. Confirm the Pod is running:

```bash
kubectl get pods
```

3. Check sidecar logs:

```bash
kubectl logs pod-practice -c siedcar-container
```

4. Access nginx:

```bash
kubectl port-forward pod/pod-practice 8080:80
curl http://localhost:8080
```

---

## 🔎 Expected Behavior

* The **sidecar container** writes `<h1>Hello from sidecar</h1>` to `index.html` inside the shared volume.
* The **nginx container** serves this file from `/usr/share/nginx/html`.
* When you access the Pod, you should see:

```html
<h1>Hello from sidecar</h1>
```

---

## 📖 Key Concepts Practiced

* Multi-container Pods
* Sidecar pattern in Kubernetes
* `emptyDir` volume for communication between containers
* Mounting an **NFS volume** for persistent shared storage
* Liveness probes for container health monitoring

---
![alt text](<Screenshot from 2025-08-31 22-50-27.png>)