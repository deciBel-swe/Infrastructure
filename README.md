# deciBel Infrastructure

This repository contains the Kubernetes orchestration and configuration for the **deciBel** project.

## Project Structure
* **k8s-manifests/**: Application-level resources (Frontend, Backend, and Ingress routing).
* **k8s-config/**: Cluster-level infrastructure tools and security configurations.

## Prerequisites
* A running Kubernetes cluster (k3s used for this project).
* Helm v3+ installed.
* A domain name pointing to your Node's Public IP.

## Deployment Order
1. **Initialize Infrastructure**: Follow instructions in `k8-config/README.md` to install the controllers.
2. **Deploy Application**: Run `kubectl apply -f k8s-manifests`.