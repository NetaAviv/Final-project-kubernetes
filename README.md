# Neta's Final Project - WordPress Deployment on Kubernetes using helm!

This project provides a Helm chart for deploying a WordPress application with a MySQL database and monitoring in grafana on a Kubernetes cluster.

## Prerequisites

Before you start, make sure you have:
- A running Kubernetes cluster (Minikube, EKS, or any other Kubernetes provider)
- Helm installed
- kubectl installed and configured
- Git
  
## Installation Steps

### 1. Add the Kubernetes Context
Ensure you are connected to the correct Kubernetes cluster:

```sh
kubectl config current-context  # Check current cluster
kubectl get nodes  # Verify cluster is running
```

### 2. Download and Extract the Helm Chart

Clone this repository or download the `.tgz` Helm package:

```sh
git clone https://github.com/NetaAviv/Final-project-kubernetes.git
cd Final-project-kubernetes
```

### 3️⃣ Install the Helm Chart

Run the following command to deploy the application:

```sh
helm install my-wordpress my-wordpress-app-for-helm-0.1.0.tgz
```

This will install:
- MySQL as a StatefulSet with a Persistent Volume
- WordPress as a Deployment with Persistent Storage
- Kubernetes Services to expose MySQL and WordPress
- An Ingress resource for external access (if configured)

### 4️⃣ Verify the Deployment

Check if the pods, services, and ingress are running:

```sh
kubectl get pods
kubectl get svc
kubectl get ingress
```

### 5️⃣ Access the WordPress Site

If you have an Ingress controller set up, get the external URL:

```sh
kubectl get ingress
```

If using NodePort, find the exposed port:

```sh
kubectl get svc my-wordpress -o=jsonpath='{.spec.ports[0].nodePort}'
```

Then, access WordPress in your browser using `http://<your-cluster-ip>:<NodePort>`

## Uninstalling the Helm Chart

To delete the deployment and free resources:

```sh
helm uninstall my-wordpress
```

## Customizing the Deployment

Modify the `values.yaml` file before installing to change settings like:
- **MySQL Passwords**
- **Storage Class**
- **Replica Counts**
- **Ingress Configuration**

Then install using:

```sh
helm install my-wordpress -f values.yaml my-wordpress-app-for-helm-0.1.0.tgz
```

## Troubleshooting

- Check logs if pods are failing:
  ```sh
  kubectl logs -l app=wordpress
  kubectl logs -l app=mysql
  ```
- Describe failing pods for errors:
  ```sh
  kubectl describe pod <pod-name>
  ```
- If ingress is not working, ensure the NGINX Ingress Controller is installed:
  ```sh
  kubectl get pods -n kube-system | grep ingress
  ```

---

Now your WordPress app should be up and running on Kubernetes using Helm! 

