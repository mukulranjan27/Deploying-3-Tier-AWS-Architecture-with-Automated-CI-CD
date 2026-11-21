# Deploying-3-Tier-AWS-Architecture-with-Automated-CI-CD
Production-Ready 3-Tier AWS Architecture with Multi-AZ High Availability, React, Node.js, RDS MySQL, NGINX, and Automated CI/CD using AWS CodePipeline, CodeBuild &amp; CodeDeploy.
# 🚀 Highly Available 3-Tier AWS Architecture with Automated CI/CD

A production-oriented, highly available and scalable 3-tier web application deployed on AWS using a multi-AZ architecture, automated CI/CD pipelines, load balancing, centralized monitoring, and secure networking.

This project demonstrates how to design, deploy, and manage a modern cloud infrastructure using AWS services and DevOps practices.

---

## 📌 Project Overview

The project implements a highly available 3-tier architecture consisting of:

- **Presentation Tier:** React frontend served through NGINX on Amazon EC2.
- **Application Tier:** Node.js backend application managed by PM2 on Amazon EC2.
- **Database Tier:** Amazon RDS for MySQL with primary and standby database instances.

The infrastructure is distributed across two Availability Zones to improve availability and fault tolerance.

Separate CI/CD workflows automate frontend and backend deployments using AWS CodePipeline, CodeBuild, CodeDeploy, and Amazon S3.

## 🏗️ Architecture Diagram

![AWS 3-Tier Architecture](docs/architecture.png)

> Add your architecture image to the repository under `docs/architecture.png` to display it here.

---

## 🛠️ AWS Services & Technologies Used

| Category | Services / Technologies |
|---|---|
| Cloud Platform | AWS |
| Compute | Amazon EC2 |
| Networking | Amazon VPC, Public & Private Subnets |
| Load Balancing | Application Load Balancer (ALB) |
| Content Delivery | Amazon CloudFront |
| DNS Management | Amazon Route 53 |
| SSL/TLS | AWS Certificate Manager (ACM) |
| Frontend | React.js, NGINX |
| Backend | Node.js, PM2 |
| Database | Amazon RDS MySQL |
| CI/CD | AWS CodePipeline, CodeBuild, CodeDeploy |
| Artifact Storage | Amazon S3 |
| Monitoring | Amazon CloudWatch |
| Logging | CloudWatch Logs |
| Tracing | AWS X-Ray |
| Security | IAM, Security Groups, AWS Secrets Manager |
| Notifications | Amazon SNS, Amazon SES, Slack, Microsoft Teams |

---

## 🏛️ Infrastructure Architecture

### 1. Networking Layer

- Created a custom Amazon VPC.
- Configured two Availability Zones for high availability.
- Created public subnets for internet-facing resources.
- Created private subnets for application and database resources.
- Configured Application Load Balancers for traffic distribution.
- Restricted communication between tiers using Security Groups.

### 2. Presentation Tier

**Technologies:** React.js, NGINX, Amazon EC2

- Deployed the frontend application on EC2 instances.
- Configured NGINX as a web server and reverse proxy.
- Configured API forwarding from `/api/` to the internal Application Load Balancer.
- Implemented an HTTP health-check endpoint.
- Configured CloudWatch Agent to collect NGINX access and error logs.

### 3. Application Tier

**Technologies:** Node.js, PM2, Amazon EC2

- Deployed the backend Node.js application on private EC2 instances.
- Used PM2 for process management.
- Configured PM2 to restore application processes after system reboot.
- Integrated the backend with the RDS MySQL database.
- Configured centralized application and error logging using CloudWatch Agent.

### 4. Database Tier

**Technology:** Amazon RDS MySQL

- Deployed Amazon RDS MySQL in the private database subnet.
- Configured primary and standby database deployment across Availability Zones.
- Enabled database replication through the RDS Multi-AZ configuration.
- Restricted database access to authorized application-tier Security Groups.

---

## 🔄 CI/CD Pipeline Architecture

The project implements separate CI/CD pipelines for frontend and backend applications.

**CI/CD workflow:**

```text
                  GitHub
                     |
              CodePipeline
                     |
              Source Stage
                     |
              Build Stage
                     |
               CodeBuild
                     |
             Build Artifacts
                     |
                Amazon S3
                     |
               Deploy Stage
                     |
              CodeDeploy
                     |
                Amazon EC2
                     |
             Application Update
```

### Frontend CI/CD Pipeline

1. Developer pushes frontend changes to GitHub.
2. AWS CodePipeline detects the new commit.
3. CodeBuild retrieves the source code.
4. Dependencies are installed and the frontend application is built.
5. Build artifacts are stored in Amazon S3.
6. CodeDeploy deploys the updated frontend application to the presentation-tier EC2 instances.
7. NGINX serves the updated frontend application.

### Backend CI/CD Pipeline

1. Developer pushes backend changes to GitHub.
2. AWS CodePipeline triggers the backend pipeline.
3. CodeBuild retrieves the backend source code.
4. Dependencies are installed and the application build is prepared.
5. Build artifacts are stored in Amazon S3.
6. CodeDeploy deploys the updated backend application to application-tier EC2 instances.
7. PM2 manages the backend application process.

---

## 📊 Monitoring & Observability

Implemented centralized monitoring and logging to improve application visibility and troubleshooting.

**Amazon CloudWatch**
- Monitors EC2 infrastructure.
- Collects NGINX access and error logs.
- Collects Node.js application logs.
- Collects CodeDeploy agent and deployment logs.
- Supports CloudWatch alarms for infrastructure monitoring and scaling actions.

**AWS X-Ray**
- Provides application tracing capabilities.
- Helps identify application performance bottlenecks.

---

## 🔐 Security Implementation

Security is implemented through multiple layers:

- **IAM Roles & Policies:** Control access to AWS resources.
- **Security Groups:** Restrict communication between infrastructure tiers.
- **Private Subnets:** Keep application and database resources isolated from direct public access.
- **AWS ACM:** Manages SSL/TLS certificates.
- **AWS Secrets Manager:** Provides centralized secrets management.
- **HTTPS:** Secures application communication.
- **Database Isolation:** Restricts database connectivity to authorized application resources.

---

## 📬 Notification & Alerting

The architecture incorporates notification integrations for operational visibility.

- Amazon SNS
- Amazon SES
- Slack
- Microsoft Teams

These integrations can be used for deployment notifications, operational alerts, and infrastructure events.

---

## 📂 Project Structure

The repository can be organized as follows:

```text
aws-3-tier-cicd-project/
│
├── frontend/
│   ├── appspec.yml
│   ├── buildspec.yml
│   └── scripts/
│
├── backend/
│   ├── appspec.yml
│   ├── buildspec.yml
│   └── scripts/
│
├── deployment/
│   ├── web-tier-user-data.txt
│   └── app-tier-user-data.txt
│
├── docs/
│   └── architecture.png
│
└── README.md
```

---

## ⚙️ Deployment Configuration

### Frontend EC2 Configuration

The frontend deployment script performs the following tasks:

- Updates system packages.
- Installs and configures NGINX.
- Configures NGINX as a reverse proxy.
- Sets up the `/api/` endpoint to forward requests to the internal application-tier ALB.
- Configures a health-check endpoint.
- Installs the CloudWatch Agent.
- Configures frontend and CodeDeploy log collection.
- Installs and starts the CodeDeploy agent.

### Backend EC2 Configuration

The backend deployment script performs the following tasks:

- Updates system packages.
- Installs Node.js 20.
- Installs PM2 globally.
- Creates application log directories.
- Configures PM2 startup and process persistence.
- Installs the CloudWatch Agent.
- Configures backend and CodeDeploy log collection.
- Installs and starts the CodeDeploy agent.

---

## 🎯 Key Benefits

- **High Availability:** Multi-AZ architecture improves infrastructure resilience.
- **Scalability:** Load balancers and CloudWatch alarms support scaling strategies.
- **Automation:** Separate frontend and backend CI/CD pipelines automate deployments.
- **Security:** Private subnets, IAM, Security Groups, and secrets management.
- **Monitoring:** Centralized application and infrastructure logs.
- **Reliability:** PM2 process management and RDS Multi-AZ deployment.
- **Maintainability:** Independent frontend and backend deployment workflows.

---

## 📚 Skills Demonstrated

- AWS Cloud Architecture
- Amazon VPC & Subnet Design
- EC2 & Application Load Balancer
- Route 53 & CloudFront
- AWS CodePipeline
- AWS CodeBuild
- AWS CodeDeploy
- Amazon S3
- Amazon RDS MySQL
- NGINX Reverse Proxy
- Node.js & PM2
- CloudWatch Monitoring
- IAM & Security Groups
- AWS Secrets Manager
- Linux Administration
- CI/CD Automation
- High Availability & Fault Tolerance
- Production Infrastructure Design

---

## 🚀 Project Outcome

Designed a production-oriented 3-tier AWS infrastructure with frontend and backend separation, multi-AZ deployment, automated CI/CD workflows, centralized monitoring, and layered security.

The project demonstrates practical experience in AWS infrastructure design, application deployment automation, cloud networking, and DevOps engineering.

---

**Author:** Mukul Ranjan

**Domain:** Cloud Computing | AWS | DevOps | CI/CD

**GitHub:** [Mukul Ranjan](https://github.com/mukulranjan27)

