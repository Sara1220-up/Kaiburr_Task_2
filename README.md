

```markdown
# Task 2 — Deploy Spring Boot + MongoDB Application on Kubernetes

This is the implementation of **Task 2** from the **Kaiburr Internship**, which involves deploying the Spring Boot + MongoDB application (from Task 1) on a **Kubernetes cluster** with persistent storage and network connectivity.

## 📖 Task Description
Create Kubernetes manifests to deploy:
1. **MongoDB** as a separate pod with a persistent volume.
2. **Spring Boot Application** as another pod using the Docker image built in Task 1.
3. **Service objects** to expose both pods and allow communication between them.
4. Demonstrate that the backend API is reachable from the host machine.

## 🧩 Project Structure
```

task2/
│
├── k8s/
│   ├── mongo-pv.yaml              # Persistent Volume Claim for MongoDB
│   ├── mongo-deployment.yaml      # MongoDB Deployment + Service
│   ├── app-deployment.yaml        # Spring Boot Application Deployment
│   └── app-service.yaml           # NodePort Service for external access
│
├── screenshots/                   # Screenshots of pods, services, curl results
└── README.md

````

## ⚙️ Prerequisites
Before deploying, ensure the following are installed and running:
- **Docker Desktop** or **Minikube** with Kubernetes enabled  
- **kubectl** CLI configured  
- The Docker image from Task 1 (`kaiburr-taskmanager:latest`) is available locally or on Docker Hub  

## 🧠 Kubernetes Deployment Steps
1. **Start Kubernetes cluster**
   ```bash
   minikube start
````

2. **Apply manifests**

   ```bash
   kubectl apply -f k8s/
   ```
3. **Verify deployments and services**

   ```bash
   kubectl get pods
   kubectl get svc
   ```
4. **Access the application**

   * Find the NodePort service URL:

     ```bash
     minikube service taskmanager-service --url
     ```
   * Test endpoint:

     ```bash
     curl http://<NodePort-URL>/tasks
     ```
5. **Check MongoDB persistence**

   * Delete and recreate the Mongo pod:

     ```bash
     kubectl delete pod <mongo-pod-name>
     kubectl get pods
     ```
   * Data should remain intact because of the PVC (`mongo-pv-claim`).

## 🧾 YAML Manifest Overview

### 🗄️ mongo-pv.yaml

Defines a **PersistentVolumeClaim** for MongoDB data storage.

### 🧱 mongo-deployment.yaml

Creates the MongoDB **Deployment** (1 replica) and its **Service** for internal access (`mongo-service`).

### 🧩 app-deployment.yaml

Deploys the **Spring Boot application** container (`kaiburr-taskmanager:latest`) with environment variable:

```yaml
SPRING_DATA_MONGODB_URI=mongodb://mongo-service:27017/tasksdb
```

### 🌐 app-service.yaml

Exposes the application through a **NodePort (30080)** so it can be accessed from the host machine.

## 📸 Screenshots

Include screenshots with **system date/time** and **your name visible**:

| Action                      | Screenshot                                |
| --------------------------- | ----------------------------------------- |
| Pods running                | ![pods](Deployments,Services/Pods/andMongoLogs.png)         |
| Services running            | ![services](cURLCommands/using/BusyBoxTest) |
| Application endpoint output | ![curl](cURLCommands/using/BusyBoxTest)         |
| MongoDB data persisted      | ![db](tasksdb)          |

## ✅ Validation Checklist

* [x] Both MongoDB and Spring Boot pods deployed successfully
* [x] Services created and accessible
* [x] Data persists after pod deletion
* [x] Screenshots attached with name and timestampcURL Commands_using_BusyBox_Test


```
