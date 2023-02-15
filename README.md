# Kubernetes Manifests

## Description of the application
#### This application is designed with a microservices architecture that consists of three main components: frontend, backend, and RDS for Postgres database. The frontend component is responsible for providing an intuitive and user-friendly interface for users to interact with the application. The backend component is the core of the application, where all business logic is implemented and communicates with the database to store and retrieve data. The RDS database is used to store and manage the application's data in a scalable and secure manner. Each component operates independently, allowing for a more flexible and efficient development process.
<br>

## Instructions

### 1. Creating K8s secret
Database instance should be already running and created with a custom user, password and database.

Provide base64 encoded user, password, host and database in the [external-postgres-secret.yml](./external-postgres-secret.yml)
and apply it to create K8s secret.
```
kubectl apply -f external-postgres-secret.yml
```
<br>

### 2. Deploy the backend application with an ingress. 
In the [backend-deployment.yml](./backend-deployment.yml) file at the line _17_, change to your backend image. Apply backend* files and the file with ingress configurations.
<br>

### 3. Connect Backend to the Frontend

### Install Nginx Ingress Controller 
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx
```