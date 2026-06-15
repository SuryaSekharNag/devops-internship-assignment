# DevOps Internship Assignment

## Part 1: Git \& SSH

### Difference Between Git Fetch and Git Pull

\# Git pull only downloads changes from remote repository (from feature-B)

* git fetch downloads changes from the remote repository but does not merge them.
* git pull downloads changes and automatically merges them into the current branch.

### Merge Conflict Resolution

A merge conflict occurs when two branches modify the same line of a file.

Steps to resolve:

1. Open the conflicting file.
2. Remove conflict markers.
3. Keep the required changes.
4. Save the file.
5. Run:
git add .
git commit -m "Resolved merge conflict"

## Part 2: Docker

### Dockerfile vs Image vs Container

* Dockerfile: Instructions to build an image.
* Docker Image: Template created from a Dockerfile.
* Docker Container: Running instance of an image.

### Reducing Docker Image Size

* Use slim/alpine images.
* Remove unnecessary files.
* Use .dockerignore.
* Use multi-stage builds.

## Part 3: Kubernetes

### Pod

Smallest deployable unit in Kubernetes.

### Deployment

Manages Pods and scaling.

### Service

Exposes Pods to users and other services.

### Why EKS?

EKS is a managed Kubernetes service where AWS manages the control plane, upgrades, and availability.

## Part 4: CI/CD

GitHub Actions is used to:

* Build Docker images.
* Run tests.
* Push images.
* Deploy applications automatically.

## Part 5: Monitoring

### Metrics

CPU, Memory, Network usage.

### Logs

Application events and error messages.

### Traces

Track requests across multiple services.

## Monitoring Tools

* Prometheus
* Grafana
* CloudWatch
* ELK Stack

## Part 6: Scenario

1. Clone code from GitHub.
2. Containerize using Docker.
3. Push image to DockerHub/ECR.
4. Deploy to Kubernetes/EKS.
5. Configure CI/CD pipeline.
6. Configure logging and monitoring.
7. Verify deployment.

