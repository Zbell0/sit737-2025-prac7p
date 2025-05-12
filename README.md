# CloudNotes Application with MongoDB on Kubernetes

This repository contains a containerized CloudNotes To-Do application with MongoDB integration deployed on Kubernetes. The application demonstrates proper database connectivity, persistent storage, and Kubernetes secret management.



## Prerequisites

To deploy and interact with this application, you need:

- Kubernetes cluster (Minikube, Docker Desktop with Kubernetes, or a cloud-based service like GKE)
- kubectl (Kubernetes command-line tool)
- Git
- Web browser

## Architecture Overview

The application consists of:
- Frontend/Backend CloudNotes application
- MongoDB database with persistent storage
- Kubernetes secrets for secure credential management
- Kubernetes services for networking

## How to Access and Use the Deployed CloudNotes App

This section provides detailed instructions for accessing and interacting with the CloudNotes To-Do app deployed on a Kubernetes cluster.

### Prerequisites

- Kubernetes cluster is running (locally with Minikube, Docker Desktop, or remotely on GKE).
- All required Kubernetes resources have been applied:
  - `todo-deployment.yaml`
  - `todo-app-svc.yaml`
  - `todo-mongo-deploy.yaml`
  - `todo-mongo-svc.yaml`
  - `todo-mongo-secret.yaml`
  - `PersistentVolume` and `PersistentVolumeClaim` for MongoDB
- MongoDB and app pods are in `Running` status:
  ```bash
  kubectl get pods -n cloudnotes
  ```

### Step-by-Step Access Instructions

**1. Confirm the Application Service Exists**
```bash
kubectl get svc -n cloudnotes
```
You should see a service like:
```
NAME             TYPE        CLUSTER-IP     PORT(S)
todo-app-svc     ClusterIP   10.XXX.XX.X    3000/TCP
```

**2. Port-forward the App Service to Localhost**
Use port-forwarding to access the app in your browser:
```bash
kubectl port-forward svc/todo-app-svc 3000:3000 -n cloudnotes
```
Leave this terminal open while using the app.

**3. Open the App in Your Browser**
Visit:
```
http://localhost:3000/todos
```
You should see the CloudNotes To-Do web interface.

## Application Features

| Feature | Description |
|---------|-------------|
| Add a Note | Enter a task in the input field and click Add |
| Edit Note | Click the Edit button beside a task to modify its content |
| Delete | Click the × button to remove a task |

Each action communicates with the backend Express API and stores data in the MongoDB instance running in your Kubernetes cluster.

## MongoDB Integration

The application connects to MongoDB using environment variables set in the deployment configuration. The MongoDB connection string is stored securely as a Kubernetes secret and injected into the application container.

### MongoDB Connection Verification

To confirm that the app is properly communicating with MongoDB:
```bash
kubectl logs deploy/todo-deployment -n cloudnotes
```

You should see:
```
✅ MongoDB connected
Server is on
```


