# 🚀 Kubernetes Learning Project

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com/)
[![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org/)

> A comprehensive Kubernetes learning project featuring nginx deployments, services, and multi-node Kind cluster configuration.

## 📋 Table of Contents

- [🎯 Overview](#-overview)
- [🏗️ Architecture](#️-architecture)
- [📁 Project Structure](#-project-structure)
- [🚀 Quick Start](#-quick-start)
- [📚 Learning Objectives](#-learning-objectives)
- [🛠️ Prerequisites](#️-prerequisites)
- [⚡ Deployment Guide](#-deployment-guide)
- [🔍 Verification](#-verification)
- [🌐 Access Methods](#-access-methods)
- [📖 Resource Explanations](#-resource-explanations)
- [🎓 What I Learned](#-what-i-learned)
- [🔧 Troubleshooting](#-troubleshooting)

## 🎯 Overview

This project demonstrates a complete Kubernetes setup with:
- **Multi-node Kind cluster** (1 control-plane + 3 workers)
- **Nginx deployment** with multiple replicas
- **Service discovery** and load balancing
- **Various Kubernetes workload types**
- **Persistent storage** configurations
- **External access** via port-forwarding

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Internet/Browser                         │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                EC2 Security Group                           │
│                   Port 81                                   │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│              kubectl port-forward                           │
└─────────────────────┬───────────────────────────────────────┘
                      │
┌─────────────────────▼───────────────────────────────────────┐
│                Kind Cluster                                 │
│  ┌─────────────────┐  ┌─────────────────┐                  │
│  │  Control Plane  │  │   Worker Nodes  │                  │
│  │                 │  │   (3 nodes)     │                  │
│  └─────────────────┘  └─────────────────┘                  │
│           │                     │                          │
│  ┌────────▼─────────────────────▼────────┐                 │
│  │         Nginx Service                 │                 │
│  │        (ClusterIP)                    │                 │
│  └────────┬──────────────────────────────┘                 │
│           │                                                │
│  ┌────────▼────────┐ ┌─────────────────┐ ┌──────────────┐  │
│  │   Nginx Pod 1   │ │   Nginx Pod 2   │ │ Nginx Pod N  │  │
│  │   (Running)     │ │   (Running)     │ │  (Running)   │  │
│  └─────────────────┘ └─────────────────┘ └──────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## 📁 Project Structure

```
kubernetes-learning-project/
├── 📁 nginx/                          # Nginx Kubernetes configurations
│   ├── 📄 deployment.yml              # Nginx deployment with replicas
│   ├── 📄 service.yml                 # ClusterIP service for load balancing
│   ├── 📄 pod.yml                     # Standalone nginx pod
│   ├── 📄 replicasets.yml             # ReplicaSet configuration
│   ├── 📄 deamonsets.yml              # DaemonSet (runs on every node)
│   ├── 📄 job.yml                     # One-time job execution
│   ├── 📄 cron-job.yml                # Scheduled job execution
│   ├── 📄 namespace.yml               # Namespace isolation
│   ├── 📄 persistentVolume.yml        # Storage volume definition
│   └── 📄 persistentVolumeclaim.yml   # Storage claim for pods
├── 📁 kind-cluster/                   # Kind cluster configuration
│   └── 📄 config.yml                  # Multi-node cluster setup
├── 📄 install.sh                      # Automated installation script
└── 📄 README.md                       # This documentation
```

## 🚀 Quick Start

### 1️⃣ Install Prerequisites
```bash
# Make installation script executable and run
chmod +x install.sh
./install.sh
```

### 2️⃣ Create Kind Cluster
```bash
# Create multi-node cluster
kind create cluster --config kind-cluster/config.yml --name nginx-cluster

# Verify cluster is running
kubectl cluster-info
kubectl get nodes
```

### 3️⃣ Deploy Nginx Application
```bash
# Create namespace first
kubectl apply -f nginx/namespace.yml

# Deploy all nginx configurations
kubectl apply -f nginx/

# Verify deployments
kubectl get all -n nginx
```

### 4️⃣ Access the Application
```bash
# Port forward to access nginx
kubectl port-forward service/nginx-service -n nginx 8080:80 --address=0.0.0.0

# Access via browser: http://localhost:8080
# Or from external: http://YOUR-IP:8080
```

## 📚 Learning Objectives

This project covers the following Kubernetes concepts:

- ✅ **Cluster Management** - Multi-node Kind cluster setup
- ✅ **Deployments** - Managing application deployments with replicas
- ✅ **Services** - Service discovery and load balancing
- ✅ **Pods** - Basic container orchestration
- ✅ **ReplicaSets** - Ensuring desired number of pod replicas
- ✅ **DaemonSets** - Running pods on every node
- ✅ **Jobs & CronJobs** - Batch workloads and scheduled tasks
- ✅ **Namespaces** - Resource isolation and organization
- ✅ **Persistent Storage** - Data persistence in containers
- ✅ **Port Forwarding** - External access to cluster services
- ✅ **YAML Configurations** - Infrastructure as Code

## 🛠️ Prerequisites

- **Docker** - Container runtime
- **Kind** - Kubernetes in Docker
- **kubectl** - Kubernetes CLI tool
- **Git** - Version control (for cloning)

> 💡 **Tip**: The `install.sh` script automatically installs all prerequisites!

## ⚡ Deployment Guide

### Step 1: Environment Setup
```bash
# Clone the repository
git clone https://github.com/Deexxtteerr/Kuberenetes.git
cd Kuberenetes

# Install tools (Docker, Kind, kubectl)
chmod +x install.sh && ./install.sh

# Verify installations
docker --version
kind --version
kubectl version --client
```

### Step 2: Cluster Creation
```bash
# Create cluster with custom configuration
kind create cluster --config kind-cluster/config.yml --name nginx-cluster

# Set kubectl context
kubectl cluster-info --context kind-nginx-cluster
```

### Step 3: Application Deployment
```bash
# Deploy in order
kubectl apply -f nginx/namespace.yml
kubectl apply -f nginx/deployment.yml
kubectl apply -f nginx/service.yml

# Optional: Deploy other resources
kubectl apply -f nginx/  # Deploys everything
```

## 🔍 Verification

### Check Cluster Status
```bash
# Verify nodes are ready
kubectl get nodes -o wide

# Check system pods
kubectl get pods -n kube-system
```

### Verify Nginx Deployment
```bash
# Check nginx namespace resources
kubectl get all -n nginx

# Verify service endpoints
kubectl get endpoints -n nginx

# Check pod logs
kubectl logs -n nginx -l app=nginx

# Describe service for details
kubectl describe svc nginx-service -n nginx
```

### Test Connectivity
```bash
# Test internal connectivity
kubectl run test-pod --rm -i --tty --image=busybox -- /bin/sh
# Inside pod: wget -qO- nginx-service.nginx.svc.cluster.local

# Test external access
curl localhost:8080  # After port-forward
```

## 🌐 Access Methods

### Method 1: Port Forwarding (Recommended for Development)
```bash
kubectl port-forward service/nginx-service -n nginx 8080:80 --address=0.0.0.0
# Access: http://localhost:8080
```

### Method 2: NodePort Service
```bash
# Modify service.yml to use NodePort
kubectl patch svc nginx-service -n nginx -p '{"spec":{"type":"NodePort"}}'
# Access: http://localhost:NODE_PORT
```

### Method 3: Ingress (Advanced)
```bash
# Install ingress controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
# Configure ingress resource
```

## 📖 Resource Explanations

### 🔄 Deployment (`deployment.yml`)
- Manages nginx pods with desired replica count
- Provides rolling updates and rollback capabilities
- Ensures high availability through multiple replicas

### 🌐 Service (`service.yml`)
- Exposes nginx pods within the cluster
- Provides load balancing across multiple pods
- Enables service discovery via DNS

### 📦 Pod (`pod.yml`)
- Basic unit of deployment in Kubernetes
- Runs nginx container with specified configuration
- Demonstrates single-pod deployment

### 🔁 ReplicaSet (`replicasets.yml`)
- Ensures specified number of pod replicas
- Automatically replaces failed pods
- Usually managed by Deployments

### 👥 DaemonSet (`deamonsets.yml`)
- Runs nginx pod on every cluster node
- Useful for node-level services (logging, monitoring)
- Automatically scales with cluster nodes

### ⚡ Job (`job.yml`)
- Runs nginx container to completion
- Useful for batch processing tasks
- Ensures successful completion

### ⏰ CronJob (`cron-job.yml`)
- Scheduled execution of nginx container
- Runs based on cron schedule
- Useful for periodic tasks

### 🏠 Namespace (`namespace.yml`)
- Provides resource isolation
- Organizes cluster resources
- Enables multi-tenancy

### 💾 Persistent Storage
- **PersistentVolume**: Defines storage resource
- **PersistentVolumeClaim**: Requests storage for pods
- Enables data persistence across pod restarts

## 🎓 What I Learned

Through this project, I gained hands-on experience with:

### 🏗️ **Infrastructure Management**
- Setting up multi-node Kubernetes clusters
- Understanding cluster architecture and components
- Managing cluster resources and namespaces

### 🚀 **Application Deployment**
- Deploying applications using various Kubernetes resources
- Understanding the relationship between Deployments, ReplicaSets, and Pods
- Implementing rolling updates and scaling strategies

### 🌐 **Networking & Services**
- Service discovery and DNS resolution
- Load balancing across multiple pods
- External access patterns (port-forwarding, NodePort, LoadBalancer)

### 💾 **Storage Management**
- Persistent storage concepts and implementation
- Volume mounting and data persistence
- Storage classes and dynamic provisioning

### 🔄 **Workload Management**
- Different types of workloads (long-running, batch, scheduled)
- Job execution patterns and completion handling
- DaemonSet deployment for node-level services

### 🛠️ **DevOps Practices**
- Infrastructure as Code using YAML manifests
- Version control for Kubernetes configurations
- Automated installation and deployment scripts

## 🔧 Troubleshooting

### Common Issues and Solutions

#### 🚨 **Pods Not Starting**
```bash
# Check pod status and events
kubectl describe pod <pod-name> -n nginx
kubectl get events -n nginx --sort-by='.lastTimestamp'

# Check resource constraints
kubectl top nodes
kubectl top pods -n nginx
```

#### 🚨 **Service Not Accessible**
```bash
# Verify service endpoints
kubectl get endpoints -n nginx nginx-service

# Check service configuration
kubectl describe svc nginx-service -n nginx

# Test internal connectivity
kubectl run debug --rm -i --tty --image=busybox -- /bin/sh
```

#### 🚨 **Port Forward Issues**
```bash
# Check if port is already in use
sudo lsof -i :8080
sudo netstat -tlnp | grep :8080

# Use different port
kubectl port-forward service/nginx-service -n nginx 8081:80
```

#### 🚨 **Cluster Creation Failed**
```bash
# Clean up and retry
kind delete cluster --name nginx-cluster
docker system prune -f
kind create cluster --config kind-cluster/config.yml --name nginx-cluster
```

### 📞 **Getting Help**
- Check Kubernetes documentation: https://kubernetes.io/docs/
- Kind documentation: https://kind.sigs.k8s.io/
- Use `kubectl explain <resource>` for resource documentation
- Join Kubernetes community: https://kubernetes.io/community/

---

## 🎯 **Live Demo**

This project demonstrates a fully functional nginx deployment accessible via:
- **Local Access**: `http://localhost:8080` (with port-forward)
- **External Access**: `http://YOUR-IP:8080` (with proper security group configuration)

### 📊 **Project Stats**
- **Total YAML Files**: 10
- **Kubernetes Resources**: 8 different types
- **Lines of Code**: 300+
- **Learning Hours**: Comprehensive Kubernetes fundamentals

---

<div align="center">

**🚀 Happy Learning! 🚀**

*Built with ❤️ for Kubernetes learning*

[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Deexxtteerr/Kuberenetes)

</div>