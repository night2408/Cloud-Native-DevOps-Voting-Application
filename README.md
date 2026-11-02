# Cloud-Native DevOps Voting Application

A cloud-native voting application built using microservices, Docker, Kubernetes (GKE), Terraform, and Google Cloud Build. Implements automated CI/CD, infrastructure provisioning, container orchestration, and DevSecOps practices on Google Cloud Platform.

**Developed by:** Krupa and Nishita Solanki  
**Project Date:** October 2025

---

## 1. Project Overview

This project demonstrates the design and deployment of a cloud-native voting application using modern DevOps principles and Google Cloud Platform (GCP).

The application follows a microservices architecture consisting of five independent services that communicate through a message queue and database to process votes and display results in real time.

The infrastructure is provisioned using Terraform, applications are containerized using Docker, and services are orchestrated using Google Kubernetes Engine (GKE). A GitHub-integrated Google Cloud Build pipeline automates the build and deployment process, incorporating code quality checks and container vulnerability scanning.

The primary objective is to demonstrate an automated, scalable, maintainable, and secure cloud deployment workflow.

## 2. System Architecture

The application consists of five independent components:

| Component | Technology | Responsibility |
|---|---|---|
| Vote Application | Python, Flask | Provides the interface for users to submit votes. |
| Message Queue | Redis | Temporarily stores incoming votes for asynchronous processing. |
| Background Worker | .NET | Processes queued votes and stores them in the database. |
| Database | PostgreSQL | Persists voter information and voting records. |
| Result Application | Node.js, Express, Socket.IO | Retrieves voting statistics and broadcasts real-time updates. |

### 2.1 Application Workflow

The application follows the workflow below:

1. **Vote Submission:** Users access the Flask-based voting interface and submit their choices.
2. **Message Queueing:** Vote information, including a unique voter ID and selected option, is pushed into Redis.
3. **Vote Processing:** The .NET Worker continuously retrieves messages from Redis and processes incoming votes.
4. **Data Persistence:** Processed votes are stored in PostgreSQL. Existing voter records are updated when a voter submits another vote.
5. **Result Aggregation:** The Node.js Result Application retrieves aggregated voting statistics from PostgreSQL.
6. **Real-Time Updates:** Socket.IO broadcasts updated results to connected clients without requiring manual page refreshes.

## 3. Technology Stack

| Category | Technologies |
|---|---|
| Cloud Platform | Google Cloud Platform (GCP) |
| Infrastructure as Code | Terraform |
| Containerization | Docker |
| Container Orchestration | Kubernetes, GKE |
| CI/CD | Google Cloud Build |
| Source Code Management | Git, GitHub |
| Container Registry | Google Container Registry (GCR) |
| Voting Application | Python, Flask |
| Results Application | Node.js, Express, Socket.IO |
| Background Processing | .NET |
| Message Queue | Redis |
| Database | PostgreSQL |
| Configuration Management | YAML, Terraform |

## 4. DevOps Implementation

### 4.1 Infrastructure as Code

Terraform is used to define and provision cloud infrastructure through declarative configuration files.

Key implementations include:

- Provisioning a Google Kubernetes Engine cluster.
- Configuring three e2-medium nodes in the us-central1-a zone.
- Managing infrastructure through version-controlled Terraform configuration files.
- Supporting reproducible infrastructure provisioning and teardown using Terraform commands.

### 4.2 Containerization Using Docker

Docker is used to package each application component into an independent container image.

- Individual Dockerfiles are maintained for the application services.
- Application dependencies are packaged within container images.
- Containerization ensures consistent execution across different environments.

### 4.3 Container Orchestration Using Kubernetes

Google Kubernetes Engine manages the deployment and operation of the containerized services.

- Kubernetes Deployments define the desired state and replica configuration of application services.
- ClusterIP Services enable internal communication between components.
- LoadBalancer Services expose the voting and results applications externally.
- Rolling updates facilitate application upgrades by gradually replacing existing pods.

### 4.4 Continuous Integration and Continuous Deployment

Google Cloud Build is integrated with GitHub through the `cloudbuild.yaml` configuration file.

The pipeline is triggered automatically when changes are pushed to the main branch.

**Continuous Integration (CI)**

- Executes automated code quality checks using Flake8 for the Python voting application.
- Builds Docker images for the Vote, Result, and Worker applications.
- Tags and pushes the generated images to Google Container Registry.
- Prevents subsequent pipeline stages from executing when the configured quality checks fail.

**Continuous Deployment (CD)**

- Uses `kubectl` to update Kubernetes Deployments on GKE.
- Automates application rollouts after successful image builds.
- Uses Kubernetes rolling updates to replace existing application pods.

### 4.5 DevSecOps Integration

Security practices are incorporated into the development and deployment lifecycle.

- Automated linting provides an initial level of static code quality analysis.
- Google Cloud container vulnerability scanning identifies known vulnerabilities in container images and their dependencies.
- Security checks are integrated into the CI/CD workflow to identify potential risks before deployment.

## 5. Project Structure

The repository is organized into application services, deployment configurations, and infrastructure-related components.

```text
Cloud-Native-DevOps-Voting-Application/
│
├── vote/
│   ├── app.py
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── static/
│   └── templates/
│
├── result/
│   ├── server.js
│   ├── package.json
│   ├── Dockerfile
│   ├── views/
│   └── tests/
│
├── worker/
│   ├── Program.cs
│   ├── Worker.csproj
│   └── Dockerfile
│
├── seed-data/
│   ├── Dockerfile
│   └── make-data.py
│
├── healthchecks/
│
├── k8s-specifications/
│   ├── vote-deployment.yaml
│   ├── vote-service.yaml
│   ├── result-deployment.yaml
│   ├── result-service.yaml
│   ├── worker-deployment.yaml
│   ├── db-deployment.yaml
│   ├── db-service.yaml
│   ├── redis-deployment.yaml
│   └── redis-service.yaml
│
├── docker-compose.yml
├── docker-compose.images.yml
├── docker-stack.yml
└── README.md
```

## 6. Prerequisites

The following tools and configurations are required to reproduce the deployment:

- Google Cloud Platform account with an appropriately configured project.
- Google Cloud SDK.
- Terraform.
- Docker.
- Kubernetes CLI (kubectl).
- Git and GitHub account.
- Appropriate Google Cloud IAM permissions.
- Required Google Cloud APIs enabled.

## 7. Deployment Workflow

The overall deployment process consists of the following stages:

1. Configure Google Cloud authentication and select the target GCP project.
2. Initialize Terraform and provision the required cloud infrastructure.
3. Build Docker images for the application components.
4. Push container images to the configured container registry.
5. Configure Kubernetes Deployments and Services using YAML manifests.
6. Deploy the application components to Google Kubernetes Engine.
7. Integrate the GitHub repository with Google Cloud Build.
8. Configure the CI/CD trigger for changes pushed to the main branch.
9. Monitor application deployments and container vulnerability reports.

## 8. Key Features

- **Microservices Architecture:** Independent services for vote submission, processing, storage, and result visualization.
- **Infrastructure Automation:** Reproducible cloud infrastructure provisioning using Terraform.
- **Containerization:** Docker-based application packaging.
- **Container Orchestration:** Kubernetes-based deployment and management through GKE.
- **Automated CI/CD:** GitHub-triggered build and deployment pipeline using Google Cloud Build.
- **Real-Time Results:** WebSocket-based communication using Socket.IO.
- **DevSecOps Integration:** Automated code quality checks and container vulnerability scanning.
- **Scalable Deployment:** Independent services that can be managed and scaled through Kubernetes.

## 9. Learning Outcomes

This project provided practical experience in:

- Designing and implementing microservices-based cloud applications.
- Automating cloud infrastructure using Infrastructure as Code.
- Building and managing Docker containers.
- Deploying and orchestrating applications using Kubernetes.
- Implementing automated CI/CD pipelines.
- Managing asynchronous communication using Redis.
- Integrating real-time application updates using WebSockets.
- Understanding DevSecOps principles and container security.

## 10. Contributors

**Krupa**  
**Nishita Solanki**

---

## 11. License

This project is intended for educational and demonstration purposes. Refer to the applicable licenses of the original project and its dependencies before redistributing the source code.
