

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
├── Task 2/                        # Screenshots folder
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

| Action                                      | Screenshot                                                                                                     |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Pods and Mongo Deployments                  | ![app\&mongo\_deployments](Task%202/app\&mongo_deployments.png)                                                |
| Deployments, Services, Pods, and Mongo Logs | ![Deployments\_Services\_Pods\_Logs](Task%202/Deployments%2C%20Services%2C%20Pods%2C%20and%20Mongo%20Logs.png) |
| Docker Build Verification                   | ![Dockerbuild](Task%202/Dockerbuild.png)                                                                       |
| Docker Image Verification                   | ![Docker\_verification](Task%202/Docker_verification.png)                                                      |
| Docker Inspection                           | ![Docker\_Inspection](Task%202/Docker%20Inspection.png)                                                        |
| Docker Inspect Output                       | ![Docker\_Inspect\_Output](Task%202/Docker%20Inspect%20Output.png)                                             |
| Curl Task Verified                          | ![curl\_Task\_Verified](Task%202/curl_Task_Verified.png)                                                       |
| Curl Commands Using BusyBox Test            | ![cURL\_Commands\_using\_BusyBox\_Test](Task%202/cURL%20Commands_using_BusyBox_Test.png)                       |
| Tasks DB View                               | ![tasksdb](Task%202/tasksdb.png)                                                                               |

## ✅ Validation Checklist

* [x] Both MongoDB and Spring Boot pods deployed successfully
* [x] Services created and accessible
* [x] Data persists after pod deletion
* [x] Screenshots attached with name and timestamp



Would you like me to make a **version where the image links are already encoded with your GitHub repo link**, so they will load even if your folder name changes in the future?
