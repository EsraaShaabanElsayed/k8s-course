
# Kubernetes Lab 2 – Working with Labels

## 📌 Overview

This lab demonstrates how to use **Kubernetes labels** to organize and categorize Pods.
Labels are key-value pairs attached to objects such as Pods, which allow you to **group, filter, and manage workloads** effectively.

In this lab, four Pods are created, each with a different role in the application architecture:

* **Backend Service Pod**
* **Frontend Service Pod**
* **Logging Utility Pod**
* **Monitoring Utility Pod**

Each Pod is assigned labels following the **Kubernetes recommended label convention** (`app.kubernetes.io/*`).

---


## 🚀 Deploy the Pods

1. Apply the manifest:

```bash
kubectl apply -f labels-lab.yaml
```

2. Verify that Pods are created:

```bash
kubectl get pods --show-labels
```



```
NAME              READY   STATUS    RESTARTS   AGE   LABELS
backend-service   1/1     Running   0          10s   app.kubernetes.io/component=service,app.kubernetes.io/name=service,app.kubernetes.io/version=v1.0
frontend-service  1/1     Running   0          10s   app.kubernetes.io/component=service,app.kubernetes.io/name=frontend,app.kubernetes.io/version=v1.0
logging           1/1     Running   0          10s   app.kubernetes.io/component=utility,app.kubernetes.io/name=logging,app.kubernetes.io/version=v2.0
monitoring        1/1     Running   0          10s   app.kubernetes.io/component=utility,app.kubernetes.io/name=monitoring,app.kubernetes.io/version=v1.0
```

---

## 🔎 Working with Labels

### 1. List Pods with a specific label

```bash
kubectl get pods -l app.kubernetes.io/component=service
```

This will return **backend-service** and **frontend-service** Pods.

---

### 2. List Pods by application name

```bash
kubectl get pods -l app.kubernetes.io/name=logging
```

This will return only the **logging** Pod.

---

### 3. List Pods by version

```bash
kubectl get pods -l app.kubernetes.io/version=v1.0
```

This will return **backend-service**, **frontend-service**, and **monitoring** Pods.

---

### 4. Combine label selectors

```bash
kubectl get pods -l app.kubernetes.io/component=utility,app.kubernetes.io/version=v1.0
```

This will return only the **monitoring** Pod.

