# Deploy Applications with Kubernetes Deployments

This repository contains my lab notes, manifest file, and step-by-step screenshots for creating, troubleshooting, and scaling a Kubernetes Deployment using `httpd`.

---

## 1. Problem Statement

The objective of this lab is to deploy an Apache HTTP Server (`httpd:latest`) on a Kubernetes cluster using a Deployment resource named `httpd`, ensure the pods are running properly, and scale the workload.

![Problem Statement](1.%20Problem_statement.png)

---

## 2. Deployment Manifest (`deploy.yaml`)

To deploy the application declaratively, we define a Kubernetes `Deployment` manifest. A Deployment manages a ReplicaSet, which in turn ensures a specified number of identical Pod replicas are running at any given time.

### Manifest Breakdown
* **`apiVersion: apps/v1`**: Specifies the API group and version for Deployments (note: must be `apps/v1`, not `app/v1`).
* **`kind: Deployment`**: Defines the Kubernetes resource type.
* **`metadata`**: Contains identifying information such as the deployment `name` (`httpd`) and `labels`.
* **`spec.replicas`**: Sets the desired number of Pod instances.
* **`spec.selector.matchLabels`**: Tells the Deployment which Pods to manage based on matching labels.
* **`spec.template`**: The Pod blueprint containing metadata labels and container specifications (`image: httpd:latest`).

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
  labels:
    app: httpd
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: httpd:latest
```

![Deployment File Explained](2.%20deployment_file_explained.png)

---

## 3. Applying & Scaling the Deployment

Once the `deploy.yaml` file is ready, use `kubectl` commands to create the resource, verify its status, and scale the number of replicas.

### Key Commands Used

* **Apply the manifest:**
  ```bash
  kubectl apply -f deploy.yaml
  ```

* **Scale the deployment imperatively:**
  ```bash
  kubectl scale deployment httpd --replicas=3
  ```

* **Generate a YAML template quickly (Dry Run):**
  ```bash
  kubectl create deployment httpd --image=httpd:latest --dry-run=client -o yaml > deploy.yaml
  ```

* **Inspect a live resource in YAML format:**
  ```bash
  kubectl get deployment httpd -o yaml
  ```

![Kubectl Apply and Scale](3.%20kubectl-apply-f_And_%20Scale.png)

---

## 4. Verification & Task Completion

Verify that the Deployment has the expected number of ready replicas and that all Pods are in the `Running` state.

```bash
kubectl get deployments
kubectl get pods
```

### Common Troubleshooting Notes
* **`no matches for kind "Deployment" in version "app/v1"`**: Caused by a missing `s` in `apps/v1`.
* **`unknown field "maytchLabels"`**: Caused by a typo in `matchLabels` under `spec.selector`.

![Task Completed](4.%20task-completed.png)
