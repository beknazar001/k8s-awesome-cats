# Kubernetes Manifests

## Description of the application
#### This application is designed with a microservices architecture that consists of three main components: frontend, backend, and RDS for Postgres database. The frontend component is responsible for providing an intuitive and user-friendly interface for users to interact with the application. The backend component is the core of the application, where all business logic is implemented and communicates with the database to store and retrieve data. The RDS database is used to store and manage the application's data in a scalable and secure manner. Each component operates independently, allowing for a more flexible and efficient development process.
<br>

## Instructions

### Pre-requirements: Install Nginx Ingress Controller
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx
```

### 1. Creating K8s secret
Database instance should be already running and created with a custom user, password and database.

Provide base64 encoded user, password, host and database in the [external-postgres-secret.yml](./external-postgres-secret.yml)
and apply it to create K8s secret.
```
kubectl apply -f external-postgres-secret.yml
```
<br>

### 2. Deploy the backend application 
In the [backend-deployment.yml](./backend-deployment.yml) file at line _17_, change to your backend image. Apply backend* files.
```
kubectl apply -f backend-deployment.yml
kubectl apply -f backend-cluster-ip-service.yml
```
<br>

### 3. Deploy the frontend application
In the [frontend-deployment.yml](./frontend-deployment.yml) file at line _17_, change to your frontend image. Apply frontend* files to create those resources.
```
kubectl apply -f frontend-deployment.yml
kubectl apply -f frontend-cluster-ip-service.yml
```
<br>

### 4. Create ingress to expose application publically
```
kubectl apply -f ingress-service.yml
```

### 5. Check all created resources
```
kubectl get deployment backend-deployment frontend-deployment
kubectl get svc backend-cluster-ip-service frontend-cluster-ip-service
kubectl get ingress ingress-service
```

You'll be able to open and interact with the application by opening ingress external-ip from the browser <br>



<h2 style=color:orange>"You are never too old to set another goal or to dream a new dream."</h2><h3> - C.S. Lewis</h3>