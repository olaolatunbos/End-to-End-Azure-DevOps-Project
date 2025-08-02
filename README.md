# Azure DevOps and GitOps

## Overview
This project leverages Docker, Kubernetes (AKS), and GitOps with Argo CD. It features CI/CD automation using Azure Pipelines, with container images managed in Azure Container Registry.

## Architecture Diagram
<img width="803" alt="Screenshot 2025-05-04 at 00 14 11" src="https://github.com/user-attachments/assets/fc732962-8cef-4666-b1d9-89cc934122c8" />


## 🐳 Container Setup
```shell
docker compose up
```

The `vote` app will be running at [http://localhost:8080](http://localhost:8080), and the `results` will be at [http://localhost:8081](http://localhost:8081).

## Project Structure
```bash
.
├── README.md
├── healthchecks
│   ├── postgres.sh
│   └── redis.sh
├── k8s-specifications
│   ├── db-deployment.yaml
│   ├── db-service.yaml
│   ├── redis-deployment.yaml
│   ├── redis-service.yaml
│   ├── result-deployment.yaml
│   ├── result-service.yaml
│   ├── vote-deployment.yaml
│   ├── vote-service.yaml
│   └── worker-deployment.yaml
├── pipelines
│   ├── azure-pipeline-worker.yml
│   ├── azure-pipelines-result.yml
│   └── azure-pipelines-vote.yml
├── result
│   ├── Dockerfile
│   └── views
│       ├── angular.min.js
│       ├── app.js
│       ├── index.html
│       ├── socket.io.js
│       └── stylesheets
├── scripts
│   └── update-manifest.sh
├── seed-data
│   ├── Dockerfile
│   ├── generate-votes.sh
│   └── make-data.py
├── vote
│   ├── Dockerfile
│   ├── app.py
│   ├── requirements.txt
│   ├── static
│   │   └── stylesheets
│   └── templates
│       └── index.html
└── worker
    ├── Dockerfile
    ├── Program.cs
    └── Worker.csproj
```

## 🔧 Pipelines
This Azure DevOps pipeline builds and pushes a Docker image for the worker service to Azure Container Registry whenever changes are made to files in the worker/ directory. It uses a Docker task to build the image from the specified Dockerfile and tag it with the build ID. After the image is pushed, it runs a shell script (update-manifest.sh) to update the Kubernetes manifest with the new image tag, helping automate deployments. The pipeline consists of two stages: Build and Update.

## Scripts
### update-manifest.sh
This Bash script automates the process of updating a Kubernetes deployment manifest in a Git repository. It clones the specified repo into a temporary directory, modifies the container image tag in a deployment YAML file using sed, commits the change with a predefined message, pushes the update back to the repository, and then cleans up the temporary files.

## ✨ Features

- **Azure Kubernetes Service (AKS)**
  Managed Kubernetes platform in Azure for deploying and operating containerized applications at scale.
- **Azure Container Registry (ACR)**
  Private Docker registry service for building, storing, and managing container images securely in Azure.
- **Azure Pipelines**
  CI/CD service in Azure DevOps that automates the building, testing, and deployment of applications across environments.
- **Argo CD**
  Declarative GitOps tool for Kubernetes that synchronizes your cluster state with a Git repository and provides automated deployment and rollback.






