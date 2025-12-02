# ──────────────────────────────
# 1. Start fresh Minikube cluster (Docker driver)
# ──────────────────────────────
minikube delete --all --purge
minikube start --driver=docker

# Verify it is running
minikube status
kubectl get nodes

# ──────────────────────────────
# 2. Create Nginx deployment
# ──────────────────────────────
kubectl create deployment mynginx --image=nginx

# Check deployment & pod (1/1 and 1 pod)
kubectl get deployments
kubectl get pods

# ──────────────────────────────
# 3. Expose the deployment
# ──────────────────────────────
kubectl expose deployment mynginx --type=NodePort --port=80

# Check the service (you will see a port like 3xxxx)
kubectl get service

# ──────────────────────────────
# 4. Scale to 4 replicas
# ──────────────────────────────
kubectl scale deployment mynginx --replicas=4

# Verify 4 pods are running
kubectl get deployments
kubectl get pods

# ──────────────────────────────
# 5. Access the Nginx web page (2 options – choose any one)
# ──────────────────────────────
# Option A (recommended – opens automatically)
minikube service mynginx --url
# → Copy the URL it shows and open in browser → you will see Nginx welcome page

# Option B (alternative)
kubectl port-forward svc/mynginx 8081:80
# → Then open http://localhost:8081 in browser

# ──────────────────────────────
# 6. (Optional – looks great in report) Open Kubernetes Dashboard
# ──────────────────────────────
minikube dashboard
# → Browser opens automatically → take screenshot of Deployments & Pods

# ──────────────────────────────
# 7. Clean up everything (do this at the end)
# ──────────────────────────────
kubectl delete deployment mynginx
kubectl delete service mynginx
minikube stop



********************
pipeline {
    agent any

    environment {
        // Set JAVA_HOME if needed
        JAVA_HOME = "C:\\Program Files\\Eclipse Adoptium\\jdk-17.0.17.10-hotspot"
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gouniaksharareddy/MavenJavaDemo.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                bat "mvn clean compile"
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                bat "mvn test"
            }
        }
    }

    post {
        always {
            echo "Build finished"
        }
        success {
            echo "Build and tests succeeded!"
        }
        failure {
            echo "Build or tests failed!"
        }
    }
}
Dokcer cli commands to open nginx
# Step 1: Remove old container if exists
docker rm -f mynginx

# Step 2: Pull latest Nginx image
docker pull nginx:latest

# Step 3: Run Nginx container on host port 9090
docker run -dp 9090:80 --name mynginx nginx:latest

# Step 4: Verify container is running
docker ps
*****************************************************************************************************

#docker compose stps
# 1️⃣ Create a project folder and move into it
mkdir docker-activity
cd docker-activity

# 2️⃣ Create a Dockerfile
nano Dockerfile
# Add the following content in Dockerfile:
# ----------------------------------------
# FROM ubuntu:latest
# RUN apt update && apt install -y curl
# CMD ["bash"]
# ----------------------------------------
# Save and exit (Ctrl+O, Enter, Ctrl+X)

# 3️⃣ Build the Docker image
docker build -t myubuntuimage .

# 4️⃣ Check if image is created
docker images

# 5️⃣ Create a Docker Compose file
nano docker-compose.yml
# Add the following content:
# ----------------------------------------
# version: "3.8"
#
# services:
#   app1:
#     image: myubuntuimage
#     container_name: app1
#   app2:
#     image: myubuntuimage
#     container_name: app2
# ----------------------------------------
# Save and exit (Ctrl+O, Enter, Ctrl+X)

# 6️⃣ Run Docker Compose
docker-compose up -d

# 7️⃣ Check running containers
docker ps

# ✅ You should see both containers (app1 and app2) running


********************docker compose for wordporess and mysql
services:
  wordpress: # Wordpress service
    image: wordpress:latest
    ports:
      - "9080:80" # Map port 80 of the container to port 9080 of the host
    environment:
      WORDPRESS_DB_HOST: db:3306 # Database host
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    depends_on:
      - db # Ensures the db service starts first

  db: # MySQL service
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
*****************
docker compose for flask 
A. Python Code (app.py)
This is the source code for the Flask application:
from flask import Flask
app = Flask(__name__)
@app.route("/")
def home():
    return "Hello from 24BD5A0503- NEKSHASRINIVAS"
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
B. Dockerfile
This Dockerfile defines how the Flask application image (web service) should be built:
FROM python:3.10-slim
WORKDIR /app
COPY app.py /app/
RUN pip install flask
CMD ["python", "app.py"]
C. Flask/MySQL docker-compose.yml
This compose file defines two services: web (the Flask application, which is built locally) and db (the MySQL database):
version: "3.9"

services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"
**********************nagios
# Check Docker
docker --version
docker ps

# 1. Pull Nagios image
docker pull jasonrivers/nagios:latest

# 2. Run Nagios container
docker run --name nagiosdemo -p 8888:80 jasonrivers/nagios:latest

# 3. Open in browser
# http://localhost:8888
# Login → username: nagiosadmin , password: nagios

# 4. Stop Nagios container
docker stop nagiosdemo

# 5. Remove container
docker rm nagiosdemo

# 6. Remove image
docker rmi jasonrivers/nagios:latest
