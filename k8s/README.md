# Chat App Deployment on Kubernetes Using Minikube

## Overview
This document provides a step-by-step guide to deploying a full-stack chat application (frontend, backend, and MongoDB) on a Kubernetes cluster created with Minikube. By following this guide, you will:

1. Set up Minikube to run a local Kubernetes cluster.
2. Build and push Docker images for the application components.
3. Deploy MongoDB, the backend, and frontend services on Kubernetes.
4. Use port forwarding to access the application locally.

---

## Prerequisites
Ensure the following tools are installed on your system:

1. **Minikube**
   - Install and set up Minikube from [Minikube's official documentation](https://minikube.sigs.k8s.io/docs/start/).

2. **Kubectl**
   - Install the Kubernetes command-line tool from [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

3. **Docker**
   - Install Docker from [Docker's official website](https://www.docker.com/get-started/).

---

## Steps

### Step 1: Start Minikube

1. Start the Minikube cluster:
   ```bash
   minikube start
   ```

2. If needed, specify the driver (e.g., Docker):
   ```bash
   minikube start --driver=docker
   ```

3. Verify the cluster is running:
   ```bash
   kubectl get nodes
   ```

### Step 2: Build and Push Docker Images

1. Log in to Docker Hub:
   ```bash
   docker login -u <your-docker-username>
   ```

2. Build and push the backend image:
   ```bash
   docker build -t <your-docker-username>/chatapp-backend:latest .
   docker push <your-docker-username>/chatapp-backend
   ```

3. Build and push the frontend image:
   ```bash
   docker build -t <your-docker-username>/chatapp-frontend:latest .
   docker push <your-docker-username>/chatapp-frontend
   ```

### Step 3: Create Kubernetes Namespace

1. Apply the namespace configuration:
   ```bash
   kubectl create -f namespace.yml
   ```

2. Verify the namespace:
   ```bash
   kubectl get ns
   ```

### Step 4: Deploy MongoDB

1. Apply the Persistent Volume (PV) and Persistent Volume Claim (PVC):
   ```bash
   kubectl apply -f mongodb-pv.yml
   kubectl apply -f mongodb-pvc.yml
   ```

2. Deploy MongoDB:
   ```bash
   kubectl apply -f mongodb-deployment.yml
   ```

3. Verify MongoDB pods:
   ```bash
   kubectl get pods -n chat-app
   ```

4. Expose MongoDB:
   ```bash
   kubectl apply -f mongodb-service.yml
   ```

### Step 5: Deploy Backend and Frontend Services

1. Deploy the backend service:
   ```bash
   kubectl apply -f backend-service.yml
   ```

2. Deploy the backend application:
   ```bash
   kubectl apply -f backend-deployment.yml
   ```

3. Deploy the frontend service:
   ```bash
   kubectl apply -f frontend-service.yml
   ```

4. Deploy the frontend application:
   ```bash
   kubectl apply -f frontend-deployment.yml
   ```

5. Verify all services:
   ```bash
   kubectl get svc -n chat-app
   ```

   Example output:
   ```bash
   NAME       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
   backend    ClusterIP   10.111.61.151   <none>        5001/TCP   3m
   frontend   ClusterIP   10.100.166.96   <none>        80/TCP     3m
   ```

### Step 6: Access the Application

1. Use port forwarding to access the backend and frontend locally:

   - Backend:
     ```bash
     kubectl port-forward service/backend -n chat-app 5001:5001
     ```

   - Frontend:
     ```bash
     kubectl port-forward service/frontend -n chat-app 8080:80
     ```

2. Open your browser and navigate to:
   - Backend: [http://localhost:5001](http://localhost:5001)
   - Frontend: [http://localhost:8080](http://localhost:8080)

### Step 7: Apply Secrets and Ingress (Optional)

1. Apply the secrets:
   ```bash
   kubectl apply -f secrets.yml
   ```

2. Apply the ingress configuration:
   ```bash
   kubectl apply -f ingress.yml
   ```

3. Verify the ingress:
   ```bash
   kubectl get ingress -n chat-app
   ```

---

## Clean Up

1. To delete the frontend deployment:
   ```bash
   kubectl delete -f frontend-deployment.yml
   ```

2. To delete the Minikube cluster:
   ```bash
   minikube delete
   ```

---

## Conclusion

You have successfully deployed a full-stack chat application using Minikube and Kubernetes. Use this guide to further enhance and explore Kubernetes-based deployments for your projects.

