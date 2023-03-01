# Kubernetes Manifests

## Description of the application
#### This application is designed with a microservices architecture that consists of three main components: frontend, backend, and RDS for Postgres database. The frontend component is responsible for providing an intuitive and user-friendly interface for users to interact with the application. The backend component is the core of the application, where all business logic is implemented and communicates with the database to store and retrieve data. The RDS database is used to store and manage the application's data in a scalable and secure manner. Each component operates independently, allowing for a more flexible and efficient development process.
<br>

## Instructions

### Pre-requirements: 
- ### Build and push the *backend* and the *frontend* docker images to a registry
```
docker build -t <imageName> .
docker tag <imageName> <registryRepoName>
docker push <registryRepoName>
```
- ### Create User and Database in the Postgres Server
```
CREATE DATABASE <databaseName> ;
CREATE USER <username> WITH PASSWORD <password> ;
GRANT ALL PRIVILEGES ON DATABASE <databaseName> to <username> ;
```
- ### Create 'users' and 'login' tables in the Database
In order to create table, connect to the database server
```
psql -h <host> -U <username> -d <databaseName>
```

Copy and past the snipped below to create 'users' and 'login' tables
```postgres
CREATE TABLE users (
    id serial PRIMARY KEY,
    name  VARCHAR(100),
    email text UNIQUE NOT NULL,
    score BIGINT DEFAULT 0,
    joined TIMESTAMP NOT NULL
);

CREATE TABLE login (
    id serial PRIMARY KEY,
    email text UNIQUE NOT NULL,
    hash VARCHAR(100) NOT NULL
);
```
Run this command to check created table
```
\dt
```
To exit from database
```
\q
```
- ### Install Nginx Ingress Controller to the K8s Cluster
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx
```
<br>

### 1. Creating K8s secret
Database instance should be already running and created with a custom user, password and database.

Provide base64 encoded user, password, host and database in the [external-postgres-secret.yml](./external-postgres-secret.yml)
and apply it to create K8s secret.
To encoded you can use:
```bash
# replace 'credential-here' with credential to get base64 encoded
echo -n 'credential-here' | base64
```
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

### 4. Create ingress to expose application publicly
```
kubectl apply -f ingress-service.yml
```
<br>

### 5. Check all created resources
```
kubectl get deployment backend-deployment frontend-deployment
kubectl get svc backend-cluster-ip-service frontend-cluster-ip-service
kubectl get ingress ingress-service
```

You'll be able to open and interact with the application by opening ingress domain address from the browser <br><br><br>

### 6. Check points
```
1: open <ingresDomainAddress> from browser. If you are not able to open then there is problem with frontend or with Ingress Controller
2: open <ingresDomainAddress>/api ; There should be written "app is working" that means your backend is working properly. If not there is issues with setting the backend application.
3: open <ingresDomainAddress>/api/all ; There should be empty or buch of texts. If you are not able to open, then it means problems with connection to Database. Double check on credentials. If you can open, then your database connection is also set up correctly. Did great so far. Now, you can register, signing and go through the application. 
```




<h2 style=color:orange>"You are never too old to set another goal or to dream a new dream."</h2><h3> - C.S. Lewis</h3>
