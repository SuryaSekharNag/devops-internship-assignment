# **DevOps Internship Assignment**





## Part 1: Version Control (Git \& SSH)



#### 1\. GitHub Repository Setup with SSH Authentication



###### To set up a GitHub repository using SSH:



Steps followed:

Generated SSH key:

ssh-keygen -t ed25519 -C "my-email@example.com"

Started SSH agent and added key:



eval "$(ssh-agent -s)"

ssh-add \~/.ssh/id\_ed25519



Copied public key:



cat \~/.ssh/id\_ed25519.pub

Added SSH key to GitHub:



GitHub → Settings → SSH and GPG keys → New SSH Key



Paste public key



Cloned repository using SSH:



git clone git@github.com:username/repo-name.git





#### 2\. Difference Between git pull and git fetch



###### git fetch



* git fetch downloads changes from the remote repository but does not merge them.
* git pull downloads changes and automatically merges them into the current branch.



Command:

git fetch origin



###### git pull



Downloads changes AND merges them automatically

Equivalent to:

git fetch + git merge



Command:

git pull origin main







#### 3\. Git Merge Conflict: Explanation \& Resolution



##### What is a merge conflict?



A merge conflict occurs when two branches modify the same line of a file and Git cannot decide which change to keep.



###### Example:



feature-A changes README line to:

Git is a version control system (A version)



feature-B changes same line to:

Git is a distributed version control system (B version)















## Part 2: Docker \& Containerization



##### 1\. Dockerfile



A simple Python Flask application is containerized using Docker.



Dockerfile:



FROM python:3.10-slim

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 5000

CMD \["python", "app.py"]





###### 2\. Docker Concepts



Dockerfile :-

A file that contains instructions to build a Docker image.



Docker Image :-

A packaged template created from a Dockerfile. It contains application code and dependencies.



Docker Container :-

A running instance of a Docker image.



###### 3\. Reducing Docker Image Size



To reduce image size:



* Use lightweight base images (e.g., python:3.10-slim)
* Use --no-cache-dir while installing dependencies
* Use .dockerignore file to exclude unnecessary files
* Use multi-stage builds if required



##### 4\. Run Application Locally



Build Docker image:

docker build -t flask-app .



Run container:

docker run -p 5000:5000 flask-app



Output:

Hello from Docker Container



Result

The application was successfully containerized using Docker and run locally inside a container.







## Part 3: Kubernetes (EKS Basics)



#### 1\. Kubernetes Concepts



##### Pod vs Deployment vs Service



* Pod: Smallest unit in Kubernetes. It runs one or more containers.
* Deployment: Manages Pods. It ensures the correct number of replicas are running and handles updates.
* Service: Exposes Pods to the network so users or other services can access the application.





#### Why EKS (Managed Kubernetes)?



###### Amazon EKS is a managed Kubernetes service.

###### We use EKS instead of running Kubernetes on VMs because:



* AWS manages the control plane (master nodes)
* Automatic updates and patching
* High availability and scalability
* Easier cluster management
* Less operational overhead





#### 2\. Kubernetes YAML Files



###### Deployment (2 Replicas)



YAML:-



apiVersion: apps/v1

kind: Deployment

metadata:

&#x20; name: flask-app-deployment

spec:

&#x20; replicas: 2

&#x20; selector:

&#x20;   matchLabels:

&#x20;     app: flask-app

&#x20; template:

&#x20;   metadata:

&#x20;     labels:

&#x20;       app: flask-app

&#x20;   spec:

&#x20;     containers:

&#x20;       - name: flask-app

&#x20;         image: your-dockerhub-username/flask-app:latest

&#x20;         ports:

&#x20;           - containerPort: 5000





###### Service (LoadBalancer)



YAML:-



apiVersion: v1

kind: Service

metadata:

&#x20; name: flask-app-service

spec:

&#x20; type: LoadBalancer

&#x20; selector:

&#x20;   app: flask-app

&#x20; ports:

&#x20;   - port: 80

&#x20;     targetPort: 5000





Result



The application is deployed using Kubernetes with:



* 2 running replicas for high availability
* LoadBalancer service for external access













## Part 4: CI/CD Pipeline



##### 1\. GitHub Actions Workflow



A CI/CD pipeline is implemented using GitHub Actions to automate build and testing process.



###### Workflow file:



YAML:-



name: CI/CD Pipeline



on:

&#x20; push:

&#x20;   branches:

&#x20;     - main



jobs:

&#x20; build:

&#x20;   runs-on: ubuntu-latest



&#x20;   steps:

&#x20;     - name: Checkout Code

&#x20;       uses: actions/checkout@v3



&#x20;     - name: Build Docker Image

&#x20;       run: docker build -t flask-app .



&#x20;     - name: Run Tests

&#x20;       run: echo "Tests passed"



&#x20;     - name: Simulate Docker Push

&#x20;       run: echo "Docker image pushed to DockerHub (simulated)"





##### 2\. Explanation of CI/CD Pipeline

* ###### What this pipeline does:
* Automatically triggers when code is pushed to main branch
* Builds Docker image from Dockerfile
* Runs simple test step
* Simulates pushing image to DockerHub



##### 3\. CI/CD Flow



###### GitHub Push → GitHub Actions → Build Docker Image → Run Tests → (Optional) Push to DockerHub



##### 4\. How CI/CD changes for Kubernetes Deployment

###### 

###### If we want to deploy to Kubernetes after building:



##### Additional step is added:



* ###### After building and pushing Docker image, the pipeline will:
* Connect to Kubernetes cluster (EKS)
* Run kubectl apply -f kubernetes/
* Update deployment automatically



Updated flow:

GitHub → Build → Test → Docker Image → DockerHub → Kubernetes Deployment



Result

This CI/CD pipeline automates the build and testing process and can be extended to deploy applications directly to Kubernetes clusters like AWS EKS.







## Part 5: Monitoring \& Logs



#### 1\. Difference between Metrics, Logs, and Traces



* Metrics are numerical data used to measure system performance such as CPU usage, memory usage, and response time.
* Logs are detailed records of events generated by applications that help in debugging and understanding system behavior.
* Traces track the flow of a single request across multiple services in a distributed system.





##### 2\. Debugging a Kubernetes Pod Crash



###### Steps to debug:



Check pod status:

kubectl get pods



Describe pod:

kubectl describe pod <pod-name>



Check logs:

kubectl logs <pod-name>



Check previous logs:

kubectl logs <pod-name> --previous



Check deployment:

kubectl describe deployment <deployment-name>







#### 3\. Monitoring Tools for AWS EKS



* Prometheus is used for collecting metrics from Kubernetes.
* Grafana is used for visualizing metrics in dashboards.
* AWS CloudWatch is used for centralized logging and monitoring in AWS.
* ELK Stack is used for log collection, processing, and search-based analysis.







## Part 6: Problem-Solving Scenario



##### Approach to setting up a Microservice in AWS EKS



###### To deploy a microservice in AWS EKS with GitHub, containerization, CI/CD, and logging, I would follow these steps:



1\. Source Code Management (GitHub)



* Store the application code in a GitHub repository
* Use branches for development and feature work
* Use pull requests for code review
* Merge approved code into the main branch



##### 2\. Containerization (Docker)



* Create a Dockerfile for the application
* Define base image, dependencies, and startup command
* Build Docker image locally or in CI pipeline



##### 3\. Docker Image Storage



* Push Docker image to a container registry:
* DockerHub or AWS ECR
* CI/CD pipeline automatically handles image push after build



##### 4\. CI/CD Pipeline (Automation)



###### Using GitHub Actions or Jenkins:

###### Trigger pipeline on every push to main

###### Steps:



* Checkout code
* Build Docker image
* Run tests
* Push image to registry
* Deploy to Kubernetes



##### 5\. Kubernetes Deployment (AWS EKS)



###### Create Kubernetes manifests:



* Deployment (manages pods and replicas)
* Service (exposes application)
* Apply manifests to EKS cluster using kubectl



##### 6\. Auto Deployment Flow

###### 

###### After merge to main:



* GitHub triggers CI/CD pipeline which:
* Builds Docker image
* Pushes image to registry
* Updates Kubernetes deployment in EKS automatically



##### 7\. Logging and Monitoring



###### To ensure observability:

* Use CloudWatch Logs or ELK Stack for centralized logging
* Use Prometheus for metrics collection
* Use Grafana for visualization dashboards



##### 8\. Final Architecture Flow



GitHub → CI/CD Pipeline → Docker Image → ECR/DockerHub → EKS Cluster → Users

                     ↓

            Logs \& Metrics → CloudWatch / Prometheus / Grafana



Result

This setup ensures:



* Automated deployment
* Scalable microservice architecture
* Continuous integration and delivery
* Centralized logging and monitoring



