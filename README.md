# Microservices Project Example

This repository contains a microservices project showcasing a proof of concept (POC) using various technologies including VueJS, NodeJS, GO, SpringBoot, Python, and Zipkin for monitoring components.

## Project Overview

The project was cloned from the following repository: [elgris/microservice-app-example](https://github.com/elgris/microservice-app-example/tree/master).

### Features

- Utilizes Docker for containerization with Dockerfiles provided for each service.
- Includes YAML files for Kubernetes (K8s) deployment and services.
- Incorporates Zipkin for distributed tracing and monitoring.
- Added Argo CD for managing logs and monitoring components.

### Modifications

- The Docker and Kubernetes YAML files required some editing and fixing to work properly.
- Docker images were built with ARM64 architecture and pushed to Docker Hub for deployment.

## Docker

All Docker images have been pushed to Docker Hub to facilitate easy pulling by worker nodes during deployment.

## Terraform
### Infrastructure Setup
Utilized official Terraform modules for creating VPC and EKS clusters.<br>
Created a Kubernetes cluster on AWS EKS with up to 5 worker nodes using ARM64 architecture and t4g.medium instance types.

* #### Best Practices for Terraform
It is recommended to manually create an EC2 instance, assign it a role with necessary permissions, and run the Terraform code from there. This approach ensures better security and management.

## Kubernetes Deployment

### Prerequisites

- Ensure you have kubectl installed on your local machine.
- Set up Minikube for local cluster testing (optional).
- AWS CLI configured for EKS access.

### Steps

1. Local Cluster Testing with Minikube (optional)
   - `kubectl create -R -f k8s/`  # Recursively apply all deployment/service.yaml files in the k8s directory
   - `minikube service <service_name> --url`
   
2. Connecting to AWS EKS
   - `aws eks update-kubeconfig --region <region_name> --name <cluster_name>`
   - `kubectl cluster-info`

3. Deploying to EKS
   - `kubectl create -R -f k8s/`

4. Check Deployment Status
   - `kubectl get pods`     # Check the state of the pods
   - `kubectl get service`  # Check services and their URLs

## Argo CD

Argo CD has been integrated for managing logs and monitoring components within your Kubernetes cluster. Follow the steps below to set up and configure Argo CD:

### Installing Argo CD

1. Install Argo CD
   - `kubectl create namespace argocd`
   - `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

2. Download ArgoCD CLI
   - `brew install argocd`
     
3. Change the ArgoCD-Server Service Type to LoadBalancer
   - `kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'`

4. Port Forwarding to Connect to the API Server Without Exposing the Service
   - `kubectl port-forward svc/argocd-server -n argocd 8070:443`

5. Access the Argo CD UI
   - Open a web browser and navigate to `http://localhost:8070`

6. Create An Application From A Git Repository via UI

## Additional Information
Validate your Kubernetes configuration files and Terraform scripts for any architecture-specific settings.

## Conclusion
This project demonstrates the integration of various microservices using modern DevOps tools and practices, providing a solid foundation for building and deploying scalable applications.
