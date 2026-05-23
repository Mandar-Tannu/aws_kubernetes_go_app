# AWS EKS Cloud-Native KYC Platform with IRSA, ALB, RDS and S3

A production-style cloud-native KYC document management platform built using **Go (Golang)**, **Amazon EKS**, **AWS Application Load Balancer**, **Amazon RDS PostgreSQL**, **Amazon S3**, **Amazon ECR**, **IRSA**, and **Kubernetes**.

This project demonstrates end-to-end implementation of a production-style cloud-native application deployed on **Amazon Elastic Kubernetes Service (EKS)** using modern AWS-native architecture patterns.

The platform allows users to submit personal information along with KYC documents through a web interface. Uploaded documents are stored in **Amazon S3**, user metadata is persisted in **Amazon RDS PostgreSQL**, and the application is deployed as replicated Kubernetes workloads behind an **AWS Application Load Balancer (ALB)**.

The architecture follows modern production deployment principles including:

- Managed Kubernetes orchestration
- Secure IAM-based pod authentication using IRSA
- ALB ingress-based traffic routing
- ClusterIP internal service communication
- Health probe-based workload monitoring
- Kubernetes secret-based sensitive configuration
- Private database deployment
- Resource request and limit enforcement
- High availability across multiple worker nodes
- Cloud-native object storage integration

---

# Business Use Case

Many enterprise applications require secure customer onboarding workflows involving:

- Identity information collection
- Document upload handling
- Secure storage of customer documents
- Persistent relational metadata storage
- Scalable backend processing
- Secure cloud-native access control

This project simulates a simplified **Know Your Customer (KYC)** onboarding workflow where:

- Users submit personal details
- Users upload KYC verification documents
- Documents are stored in durable cloud object storage
- User metadata is stored in PostgreSQL
- Backend application scales horizontally
- Traffic is distributed across multiple replicas
- Pods securely access AWS services without static credentials

This architecture reflects real-world implementation patterns commonly used in:

- FinTech onboarding platforms
- Banking KYC systems
- Insurance onboarding workflows
- Telecom customer verification systems
- SaaS document onboarding platforms
- Internal enterprise document processing systems

---

# Key Features

## Application Features

- Web-based user registration form
- Multipart file upload support
- KYC document upload workflow
- Amazon S3 object storage integration
- PostgreSQL metadata persistence
- Automatic database schema initialization
- Structured operational logging
- Health check endpoint
- Readiness check endpoint
- Replica-aware response handling

---

## Kubernetes Features

- Amazon EKS managed Kubernetes cluster
- Managed node group deployment
- Replica-based application scaling
- Kubernetes Deployment-based lifecycle management
- ClusterIP internal service exposure
- Kubernetes Ingress resource routing
- Kubernetes Secret integration
- Liveness probes
- Readiness probes
- Pod self-healing
- Resource request and limit enforcement

---

## AWS Infrastructure Features

- Custom VPC networking
- Public subnet ALB deployment
- Private subnet worker node deployment
- Private RDS deployment
- Multi-AZ subnet architecture
- NAT Gateway outbound internet access
- Amazon EKS managed control plane
- Amazon ECR container image registry
- AWS Load Balancer Controller
- IAM Roles for Service Accounts (IRSA)
- IAM-based AWS API authentication
- Security Group-based traffic isolation

---

# Technology Stack

## Backend

- Go (Golang 1.24)
- net/http
- database/sql
- PostgreSQL driver (lib/pq)
- AWS SDK for Go v2

---

## Cloud Platform

**Amazon Web Services (AWS)**

### Services Used

- Amazon EKS
- Amazon EC2
- Amazon ECR
- Amazon RDS PostgreSQL
- Amazon S3
- Amazon VPC
- AWS IAM
- AWS Application Load Balancer
- NAT Gateway
- Security Groups

---

## Kubernetes

- Amazon EKS
- Kubernetes Deployment
- Kubernetes Service
- Kubernetes Ingress
- Kubernetes Secrets
- Kubernetes Service Accounts
- Liveness Probes
- Readiness Probes

---

## AWS Kubernetes Integrations

- AWS Load Balancer Controller
- IAM Roles for Service Accounts (IRSA)
- OIDC Provider Integration

---

## Containerization

- Docker

---

## Database

- PostgreSQL

---

## Deployment

- Amazon ECR
- Kubernetes manifests
- Helm

---

## Operating System

- Ubuntu Linux

---

# High-Level Architecture

<img width="1536" height="1024" alt="ChatGPT Image May 23, 2026, 06_11_41 PM" src="https://github.com/user-attachments/assets/cfe49b2c-25a3-4a2f-ac66-55d7db797639" />

---

# Architecture Overview

This platform is deployed on **Amazon EKS using managed Kubernetes infrastructure hosted on AWS**.

The architecture consists of the following components:

---

## 1. Internet Users

End users access the public web application through the AWS Application Load Balancer.

Responsibilities:

- Submit personal details
- Upload KYC verification documents
- Access the application through public HTTP endpoint

---

## 2. AWS Application Load Balancer (ALB)

A public-facing AWS Application Load Balancer exposes the application to internet users.

Responsibilities:

- Receive incoming HTTP traffic
- Perform health checks
- Route traffic to Kubernetes workloads
- Provide external application access
- Integrate with Kubernetes ingress resources

---

## 3. AWS Load Balancer Controller

AWS Load Balancer Controller integrates Kubernetes ingress resources with AWS ALB.

Responsibilities:

- Watch Kubernetes ingress resources
- Automatically provision ALB resources
- Register Kubernetes application targets
- Configure ALB listeners and routing rules

This removes the need for manual ALB configuration.

---

## 4. Kubernetes Ingress Layer

Kubernetes ingress defines application routing rules.

Responsibilities:

- Define HTTP routing behavior
- Connect ALB traffic to Kubernetes services
- Manage ingress-level traffic flow

Traffic flow:

```text
Internet User
   ↓
AWS ALB
   ↓
AWS Load Balancer Controller
   ↓
Kubernetes Ingress
```

---

## 5. Kubernetes Service Layer

A ClusterIP service provides internal service discovery.

Responsibilities:

- Route traffic to healthy pods
- Abstract pod networking
- Decouple workloads from dynamic pod IP changes

Traffic flow:

```text
Ingress → ClusterIP Service → Pods
```

---

## 6. Go Application Layer

The Go backend application provides:

- Frontend web form serving
- Multipart request parsing
- KYC file upload handling
- PostgreSQL integration
- Amazon S3 integration
- Structured logging
- Health monitoring endpoints

Application replicas are distributed across EKS worker nodes.

---

## 7. Amazon EKS Worker Nodes

Managed EKS worker nodes execute application workloads.

Responsibilities:

- Run application pods
- Provide container runtime execution
- Support Kubernetes scheduling
- Handle workload networking

Application replicas run across multiple nodes for resilience.

---

## 8. Amazon RDS PostgreSQL

Amazon RDS stores structured metadata.

Stored information:

- User name
- Email
- Phone number
- S3 bucket name
- S3 object key
- KYC status
- Timestamp records

RDS provides managed relational persistence.

---

## 9. Amazon S3

Amazon S3 stores uploaded KYC documents.

Benefits:

- Durable cloud object storage
- Scalable document storage
- Separation from application compute
- Secure document persistence

---

## 10. IAM Roles for Service Accounts (IRSA)

IRSA provides secure AWS authentication for Kubernetes pods.

Responsibilities:

- Eliminate static AWS credentials
- Allow pod-level IAM access
- Secure S3 access from application pods
- Follow least privilege access principles

Authentication flow:

```text
Go Application Pod
   ↓
Kubernetes Service Account
   ↓
IRSA
   ↓
IAM Role
   ↓
Amazon S3
```

---

## 11. Network Security Layer

Infrastructure is isolated using AWS networking constructs.

Components:

- Custom VPC
- Public subnets
- Private subnets
- NAT Gateways
- Security Groups

Security model:

- ALB in public subnet
- EKS worker nodes in private subnets
- RDS in private subnet
- Controlled east-west traffic
- Restricted database exposure

---

# Why This Project Matters

This project demonstrates practical implementation of:

✅ Amazon EKS managed Kubernetes deployment  
✅ Cloud-native application architecture  
✅ Kubernetes ingress architecture  
✅ AWS Load Balancer Controller integration  
✅ IAM Roles for Service Accounts (IRSA)  
✅ Secure pod-to-AWS authentication  
✅ Backend development with Go  
✅ PostgreSQL integration  
✅ Amazon S3 integration  
✅ Container image lifecycle using Amazon ECR  
✅ Health monitoring and workload resilience  
✅ Resource management and pod lifecycle control  
✅ Production-style AWS architecture patterns

---

# Project Structure

```text
aws_kubernetes_go_app/
├── go.mod
├── go.sum
├── index.html
├── main.go
├── Dockerfile
├── deployment.yaml
├── service.yaml
├── ingress-class.yaml
├── ingress.yaml
└── README.md
```

### Component Breakdown

- **go.mod** → Go module definition and dependency management
- **go.sum** → dependency checksum verification
- **index.html** → frontend user registration and KYC upload form
- **main.go** → backend application implementation
- **Dockerfile** → container image build instructions
- **deployment.yaml** → Kubernetes Deployment definition
- **service.yaml** → Kubernetes ClusterIP service definition
- **ingress-class.yaml** → Kubernetes IngressClass for AWS ALB controller
- **ingress.yaml** → Kubernetes ingress routing configuration
- **README.md** → project documentation

---

# Application Internals

The backend application is implemented in **Go (Golang)** and follows a cloud-native microservice deployment pattern.

Core responsibilities:

- Serve frontend web UI
- Accept user registration requests
- Parse multipart file uploads
- Upload documents to Amazon S3
- Store metadata in PostgreSQL
- Expose health monitoring endpoints
- Support Kubernetes lifecycle management
- Provide replica-aware operational responses

---

## Database Schema

The application automatically initializes the required PostgreSQL schema during startup.

```sql
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT NOT NULL,
    phone TEXT NOT NULL,
    document_bucket TEXT NOT NULL,
    document_key TEXT NOT NULL,
    kyc_status TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Stored Data

Each record contains:

- User identity details
- Uploaded document storage location
- KYC workflow state
- Object storage reference
- Timestamp metadata

This ensures schema readiness during application startup.

---

# HTTP Endpoints

## Frontend UI Endpoint

```http
GET /
```

Purpose:

- Serve HTML user interface
- Present registration form
- Allow KYC document submission

---

## Form Submission Endpoint

```http
POST /submit
```

Purpose:

- Accept multipart form requests
- Extract user details
- Read uploaded KYC document
- Upload document to Amazon S3
- Store metadata in PostgreSQL

---

## Health Check Endpoint

```http
GET /health
```

Purpose:

- Application health validation
- PostgreSQL connectivity verification
- Kubernetes liveness probe target
- ALB health check endpoint

Expected response:

```text
OK
```

---

## Readiness Endpoint

```http
GET /ready
```

Purpose:

- Application readiness verification
- PostgreSQL availability validation
- Kubernetes readiness probe target

Expected response:

```text
HTTP 200
```

---

# End-to-End Request Lifecycle

This section explains how a user request flows through the complete platform.

---

## Step 1 — User Access

The user accesses the application through the public AWS ALB endpoint.

Traffic flow:

```text
Internet User
   ↓
AWS Application Load Balancer
```

The ALB acts as the public entry point.

---

## Step 2 — ALB to Kubernetes Routing

AWS Load Balancer Controller observes Kubernetes ingress resources and configures ALB routing.

Traffic flow:

```text
AWS ALB
   ↓
AWS Load Balancer Controller
   ↓
Kubernetes Ingress
```

This enables automatic AWS-native ingress provisioning.

---

## Step 3 — Service Routing

Ingress forwards requests to the Kubernetes ClusterIP service.

Traffic flow:

```text
Ingress
   ↓
ClusterIP Service
```

The service provides:

- Stable service discovery
- Internal traffic routing
- Pod abstraction

---

## Step 4 — Pod Request Handling

The request reaches one healthy Go application pod.

Traffic flow:

```text
ClusterIP Service
   ↓
Go Application Pod
```

The application:

- serves frontend UI
- accepts multipart requests
- processes business logic

---

## Step 5 — Form Rendering

The application serves:

```text
index.html
```

The user sees:

- Name input
- Email input
- Phone input
- Document upload field

---

## Step 6 — Form Submission

The user submits:

- Personal details
- KYC verification document

Application receives:

```http
POST /submit
```

The Go backend:

- parses multipart request
- extracts form values
- reads uploaded file

---

## Step 7 — Amazon S3 Upload

The application uploads the document to Amazon S3.

Generated object key format:

```text
kyc-docs/YYYYMMDD-HHMMSS-filename
```

Example:

```text
kyc-docs/20260523-103015-passport.pdf
```

Benefits:

- unique object naming
- scalable cloud storage
- durable persistence

---

## Step 8 — Amazon RDS Persistence

The application inserts metadata into PostgreSQL.

Stored values:

- Name
- Email
- Phone
- S3 bucket name
- S3 object key
- KYC workflow status

This separates document storage from relational metadata.

---

## Step 9 — Response Generation

The application returns:

```text
User data stored by instance: go-app-xxxx
```

This allows verification of:

- replica distribution
- load balancing behavior
- active serving pod identity

---

# Runtime Configuration

The application depends on runtime environment variables.

Required variables:

```text
RDS_DB_HOST
RDS_DB_PORT
RDS_DB_USER
RDS_DB_PASSWORD
RDS_DB_NAME
RDS_DB_SSLMODE
S3_BUCKET_NAME
```

---

## AWS Region Configuration

Current implementation uses:

```go
config.WithRegion("ap-south-1")
```

This means:

- region is hardcoded
- AWS_REGION environment variable is currently not required

---

# Authentication Model

This project uses **IAM Roles for Service Accounts (IRSA)** instead of static AWS credentials.

Authentication flow:

```text
Go Application Pod
   ↓
Kubernetes Service Account
   ↓
OIDC Federation
   ↓
IAM Role
   ↓
Amazon S3
```

Benefits:

- no hardcoded AWS access keys
- no long-lived static credentials
- pod-level least privilege access
- production-style AWS authentication

---

# Secret Management

Sensitive values are stored using Kubernetes Secrets.

Protected data includes:

- PostgreSQL password

Example:

```yaml
RDS_DB_PASSWORD
```

This avoids hardcoding secrets inside application code or manifests.

---

# Logging Strategy

The application uses structured logging for operational visibility.

Logged events include:

- application startup
- database connection success
- schema initialization
- invalid HTTP methods
- S3 upload failures
- database insert failures
- successful user creation
- HTTP server startup

Example log:

```text
level=INFO service=go-app event=user_created
```

Benefits:

- operational debugging
- failure investigation
- observability support
- production troubleshooting

---

# Security Model

Current security controls include:

- private EKS worker nodes
- private RDS deployment
- Security Group traffic restrictions
- IRSA-based AWS authentication
- Kubernetes Secret-based password storage
- public ALB entry point only
- no direct pod exposure
- no direct database internet exposure

Security boundaries:

```text
Internet
   ↓
ALB (Public)
   ↓
EKS Application Layer (Private)
   ↓
RDS (Private)
```

This provides layered infrastructure isolation.

---

# Kubernetes Deployment Architecture

This project uses **Amazon Elastic Kubernetes Service (EKS)** for managed Kubernetes orchestration.

Kubernetes responsibilities include:

- workload scheduling
- pod lifecycle management
- replica management
- service discovery
- health monitoring
- rolling deployments
- ingress routing
- self-healing

---

## Cluster Topology

```text
Amazon EKS Managed Control Plane
        ↓
Managed Node Group
   ├── Worker Node 1
   └── Worker Node 2
```

Architecture characteristics:

- managed Kubernetes control plane
- AWS-managed cluster lifecycle
- managed worker node provisioning
- multi-node workload distribution
- private node networking

---

## Deployment Architecture

The Go application is deployed using a Kubernetes Deployment.

Deployment provides:

- replica management
- automatic failed pod replacement
- rolling updates
- declarative desired state management

Example:

```text
go-app Deployment
   ├── Pod Replica 1
   └── Pod Replica 2
```

Scaling example:

```bash
kubectl scale deployment go-app --replicas=5
```

---

## Service Architecture

The application is exposed internally using a ClusterIP service.

Purpose:

- internal service discovery
- pod traffic routing
- stable network abstraction
- ingress integration

Traffic flow:

```text
Ingress
   ↓
ClusterIP Service
   ↓
Application Pods
```

ClusterIP is used because ALB directly targets pod IPs.

---

## Ingress Architecture

Ingress provides external application exposure.

This project uses:

- Kubernetes Ingress
- AWS Load Balancer Controller
- AWS Application Load Balancer

Traffic flow:

```text
Internet User
   ↓
AWS ALB
   ↓
Ingress
   ↓
ClusterIP Service
   ↓
Pods
```

Ingress annotations provide:

- internet-facing ALB
- ALB health check integration
- IP target registration
- listener configuration

---

## ALB Target Type

This project uses:

```yaml
alb.ingress.kubernetes.io/target-type: ip
```

Traffic behavior:

```text
ALB → Pod IP directly
```

Benefits:

- avoids NodePort dependency
- cleaner architecture
- better Kubernetes-native routing
- recommended EKS ingress design

---

## Health Monitoring

### Liveness Probe

Endpoint:

```http
GET /health
```

Purpose:

- detect unhealthy containers
- restart failed application pods

---

### Readiness Probe

Endpoint:

```http
GET /ready
```

Purpose:

- validate application readiness
- verify PostgreSQL connectivity
- prevent traffic to unhealthy pods

---

## Self-Healing

If a pod fails:

```text
Desired replicas: 2
Actual running: 1
```

Kubernetes automatically creates a replacement pod.

Benefits:

- workload resilience
- automatic recovery
- reduced operational intervention

---

## Resource Controls

Deployment includes:

- CPU requests
- CPU limits
- memory requests
- memory limits

Benefits:

- workload isolation
- scheduling predictability
- resource protection
- cluster stability

---

# AWS Infrastructure Design

The platform is deployed using production-style AWS infrastructure architecture.

---

## Infrastructure Components

AWS services used:

- Amazon VPC
- Public subnets
- Private subnets
- Internet Gateway
- NAT Gateways
- Amazon EKS
- Managed Node Group
- Amazon ECR
- AWS ALB
- Amazon RDS PostgreSQL
- Amazon S3
- IAM
- IRSA
- Security Groups

---

## VPC Design

Custom VPC:

```text
10.0.0.0/16
```

Purpose:

- isolated networking
- infrastructure segmentation
- controlled routing
- secure workload separation

---

## Subnet Architecture

### Public Subnets

Used for:

- AWS Application Load Balancer
- NAT Gateways

Characteristics:

- internet accessible
- public traffic entry layer

---

### Private Subnets

Used for:

- EKS worker nodes
- Amazon RDS PostgreSQL

Characteristics:

- no direct public internet exposure
- protected application execution layer

---

## NAT Gateway Design

NAT Gateways provide outbound internet access for private resources.

Used by:

- EKS worker nodes
- package downloads
- image pulls
- AWS service communication

Benefits:

- outbound internet access
- no inbound public exposure

---

## Amazon EKS

Amazon EKS provides managed Kubernetes control plane services.

Responsibilities:

- API server management
- cluster state management
- control plane high availability
- Kubernetes version lifecycle

Benefits:

- reduced operational overhead
- managed Kubernetes infrastructure

---

## Managed Node Group

Managed worker nodes execute application workloads.

Responsibilities:

- pod execution
- container runtime hosting
- workload scheduling support

Benefits:

- simplified worker lifecycle
- AWS-managed node maintenance

---

## Amazon ECR

Amazon ECR stores container images.

Used for:

- Go application image storage
- deployment image pulls
- versioned container management

Benefits:

- private image registry
- AWS-native container lifecycle

---

## Amazon RDS PostgreSQL

Stores structured metadata.

Stored data:

- user details
- workflow state
- object references
- timestamps

Deployment characteristics:

- private subnet placement
- managed PostgreSQL engine
- controlled access

---

## Amazon S3

Stores uploaded KYC documents.

Benefits:

- durable storage
- elastic scalability
- object persistence
- application decoupling

---

## IAM and IRSA

IAM provides secure AWS authorization.

IRSA enables:

- pod identity authentication
- temporary AWS credentials
- least privilege access
- secure S3 integration

No static AWS access keys are required.

---

## Security Groups

Traffic restrictions are enforced using Security Groups.

Access model:

- ALB accepts public HTTP traffic
- EKS workloads accept ALB traffic
- PostgreSQL accepts private application traffic only

This limits backend exposure.

---

# Quick Deployment Guide

This section provides high-level deployment workflow.

---

## Prerequisites

Required:

- AWS account
- custom VPC
- Amazon EKS cluster
- managed node group
- Amazon RDS PostgreSQL
- Amazon S3 bucket
- Amazon ECR repository
- AWS Load Balancer Controller
- IRSA configuration
- kubectl
- eksctl
- Docker
- Helm

---

## Clone Repository

```bash
git clone https://github.com/Mandar-Tannu/aws_kubernetes_go_app.git
cd aws_kubernetes_go_app
```

---

## Build Docker Image

```bash
docker build -t go-app:v1 .
```

---

## Push to Amazon ECR

```bash
docker tag go-app:v1 <ACCOUNT-ID>.dkr.ecr.ap-south-1.amazonaws.com/go-app:v1
docker push <ACCOUNT-ID>.dkr.ecr.ap-south-1.amazonaws.com/go-app:v1
```

---

## Create Kubernetes Secret

Example:

```bash
kubectl create secret generic app-secrets \
  --from-literal=RDS_DB_PASSWORD=<db-password>
```

---

## Deploy Application

Apply manifests:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress-class.yaml
kubectl apply -f ingress.yaml
```

---

## Retrieve ALB Endpoint

```bash
kubectl get ingress
```

Open:

```text
http://<alb-dns-name>
```

---

# Operational Verification

---

## Verify Nodes

```bash
kubectl get nodes
```

Expected:

```text
Worker nodes Ready
```

---

## Verify Pods

```bash
kubectl get pods -o wide
```

Expected:

```text
2 Running pods
```

---

## Verify Deployment

```bash
kubectl get deployment
```

Expected:

```text
READY 2/2
```

---

## Verify Service

```bash
kubectl get svc
```

Expected:

```text
ClusterIP service
```

---

## Verify Ingress

```bash
kubectl get ingress
```

Expected:

```text
ALB DNS endpoint
```

---

## Verify Health Endpoint

```bash
curl http://<alb-dns>/health
```

Expected:

```text
OK
```

---

## Verify Readiness Endpoint

```bash
curl http://<alb-dns>/ready
```

Expected:

```text
HTTP 200
```

---

## Verify S3 Upload

Check:

```text
AWS Console → S3 → telecom-kyc-docs
```

Expected:

uploaded KYC files

---

## Verify PostgreSQL Records

Query:

```sql
SELECT * FROM users;
```

Expected:

stored user metadata

---

## Verify Self-Healing

Delete pod:

```bash
kubectl delete pod <pod-name>
```

Expected:

automatic replacement pod

---

# Security Considerations

This project implements strong production-style security foundations.

---

## Current Security Strengths

Implemented controls:

- IRSA-based AWS authentication
- no static AWS access keys
- private worker nodes
- private PostgreSQL deployment
- Kubernetes secret-based password management
- Security Group-based traffic isolation
- managed EKS control plane
- ALB-based public entry point
- no direct pod exposure
- no direct database exposure

---

## Current Limitations

Current simplifications:

- HTTP only
- no HTTPS/TLS
- database password in Kubernetes Secret
- no AWS Secrets Manager integration
- no WAF protection
- no authentication layer
- no malware scanning
- no rate limiting
- no audit logging pipeline
- single RDS deployment

---

## Production Improvements

Recommended enhancements:

- HTTPS using AWS ACM
- AWS WAF
- AWS Secrets Manager
- IAM least privilege hardening
- Multi-AZ PostgreSQL
- RDS encryption
- S3 encryption
- JWT/OAuth authentication
- file validation
- antivirus scanning
- centralized observability
- SIEM integration

---

# Future Improvements / Roadmap

Potential enhancements:

---

## Infrastructure

- Multi-AZ database deployment
- higher availability architecture
- disaster recovery design
- backup automation

---

## Kubernetes

- Horizontal Pod Autoscaler
- cluster autoscaler
- namespace isolation
- ConfigMap externalized configuration
- GitOps deployment model

---

## Observability

- Prometheus
- Grafana
- centralized logging
- distributed tracing
- alerting pipelines

---

## CI/CD

- GitHub Actions
- Jenkins pipelines
- ArgoCD
- automated testing
- deployment automation

---

## Application

- authentication layer
- duplicate validation
- business rule enforcement
- asynchronous workflow processing
- event-driven architecture
- notification workflows

---

# Project Summary

Designed and deployed a production-style cloud-native KYC platform using **Go**, **Amazon EKS**, **AWS ALB**, **Amazon RDS PostgreSQL**, **Amazon S3**, **Amazon ECR**, and **IRSA**.

Implemented secure pod-to-AWS authentication, Kubernetes ingress routing, ClusterIP service architecture, health monitoring, workload resilience, managed Kubernetes deployment, and production-style AWS infrastructure design.

This project demonstrates practical experience in:

- Amazon EKS
- Kubernetes orchestration
- IRSA
- AWS Load Balancer Controller
- cloud-native backend architecture
- Go application deployment
- AWS infrastructure engineering
- secure cloud-native design
- production deployment patterns
