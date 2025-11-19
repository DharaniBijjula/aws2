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
