A real-time electrical grid monitoring dashboard with a full DevOps CI/CD pipeline on AWS

<img width="1908" height="915" alt="Screenshot 2026-04-11 211918" src="https://github.com/user-attachments/assets/f99aa6fb-bfa1-4e9b-8746-9a2e9ffe0f84" />
<img width="1906" height="920" alt="Screenshot 2026-04-11 212015" src="https://github.com/user-attachments/assets/c777fd7f-c311-428a-8ce0-668103d5c379" />
<img width="1900" height="922" alt="Screenshot 2026-04-11 212150" src="https://github.com/user-attachments/assets/c583ec63-5d4e-41e0-bf50-f9586a516956" />
<img width="1900" height="922" alt="Screenshot 2026-04-11 212150" src="https://github.com/user-attachments/assets/70b8f89b-06bb-4d64-a213-4642d3eac040" />
<img width="1905" height="916" alt="Screenshot 2026-04-11 212248" src="https://github.com/user-attachments/assets/fb7304b9-5cb5-4169-a705-4409f7f0c880" />
<img width="1891" height="917" alt="Screenshot 2026-04-11 212308" src="https://github.com/user-attachments/assets/ffbb976e-edcd-476e-9c61-7cb446aed9fc" />

Table of Contents

-Overview
-Architecture
-Tech Stack
-Project Structure
-Application
-Infrastructure
-CI/CD Pipeline
-Monitoring
-Getting Started
-Environment Variables


Overview

ElectraVision is a graduation project that combines IoT-style electrical monitoring with a complete DevOps pipeline. It simulates real-time electrical readings (voltage, current, power, frequency, energy, power factor) and displays them on a live dashboard — all deployed automatically to AWS using a Jenkins CI/CD pipeline.

Key Features

-Real-time electrical metrics dashboard
-Live charts, alerts, and data logs
-Automated CI/CD pipeline (Jenkins)
-Containerized with Docker
-Orchestrated with Kubernetes
-Security scanning with Trivy
-Code quality analysis with SonarQube
-Infrastructure as Code with Terraform
-Monitoring with Prometheus + Grafana
-Data persistence with AWS RDS MySQL


Architecture
┌─────────────────────────────────────────────────────────────┐
│                     AWS Cloud (eu-central-1)                 │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                  VPC (10.0.0.0/16)                  │    │
│  │                                                      │    │
│  │  Public Subnet (10.0.1.0/24)                        │    │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │    │
│  │  │ Jenkins  │  │SonarQube │  │   Kubernetes     │  │    │
│  │  │t3.medium │  │t3.medium │  │   t3.medium      │  │    │
│  │  │:8080     │  │:9000     │  │   single-node    │  │    │
│  │  └──────────┘  └──────────┘  └──────────────────┘  │    │
│  │                                                      │    │
│  │  Private Subnets (10.0.10.0/24 | 10.0.11.0/24)     │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │         RDS MySQL (db.t3.micro)              │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘

Deploy Infrastructure

cd terraform_files/
terraform init
terraform plan
terraform apply -auto-approve
Destroy Infrastructure
bashterraform destroy -auto-approve

CI/CD Pipeline
The Jenkins pipeline runs automatically on every push to master.

Pipeline Stages

Clone → SonarQube Analysis → Quality Gate → Build Image
→ Trivy Scan → Push to DockerHub → Remove Image → Deploy to K8s


Monitoring

Prometheus scrapes metrics from all servers every 15 seconds.
Grafana Dashboards
DashboardIDWhat it showsNode Exporter1860CPU, RAM, Disk for all serversJenkins9964Build stats, queue, executors
Access
Prometheus : http://<monitoring-ip>:9090
Grafana    : http://<monitoring-ip>:3000

Getting Started

Prerequisites

AWS Account
Terraform >= 1.3.0
AWS CLI configured
Key pair My_Key in eu-central-1

1. Clone the repo
bashgit clone https://github.com/OmarMo20/Graduation-project.git
cd Graduation-project

3. Deploy infrastructure
bashcd terraform_files/
terraform init
terraform apply -auto-approve

5. Setup Jenkins

Open http://<jenkins-ip>:8080

Install plugins: Docker Pipeline, SonarQube Scanner, Kubernetes CLI
Add credentials: dockerhub-credentials, kubeconfig, sonarqube-token
Configure SonarQube server URL

4. Run the pipeline

Create pipeline pointing to this repo
Click Build Now

5. Access the app
http://<kubernetes-ip>:30080

