# Automated DevSecOps CI/CD Pipeline for AWS

## Overview

This project implements an automated **DevSecOps CI/CD pipeline** for a containerized Flask application deployed on AWS.

The pipeline automatically tests the application, performs security checks, builds and scans a Docker image, publishes the image to **Amazon Elastic Container Registry (ECR)**, and deploys the application to **Amazon EC2** using **AWS Systems Manager (SSM)**.

The project demonstrates how security and automated deployment can be integrated into a modern CI/CD workflow.

## Architecture

<img width="1312" height="1199" alt="image" src="https://github.com/user-attachments/assets/75d5466b-c89f-41e5-b6d1-c1a7574066e1" />


## CI/CD Workflow

Every push to the `main` branch triggers the GitHub Actions pipeline.

### 1. Application Testing

**Pytest** runs automated tests for the Flask application.

The tests verify:

* Home page response
* Health-check endpoint
* HTTP response status codes
* Expected application responses

### 2. Source Code Security Scanning

**Bandit** performs static security analysis on the Python source code.

This helps identify common security issues in Python applications before deployment.

### 3. Docker Image Build

The application is packaged into a Docker image using the project's `Dockerfile`.

The image uses:

* Python 3.12
* Flask
* A lightweight Python base image

### 4. Container Security Scanning

**Trivy** scans the Docker image for known vulnerabilities.

The pipeline is configured to fail when reportable **HIGH** or **CRITICAL** vulnerabilities are detected.

### 5. Authentication with AWS

GitHub Actions authenticates with AWS using **OpenID Connect (OIDC)**.

This avoids storing long-lived AWS access keys inside GitHub.

### 6. Image Publishing

After the security checks pass, the Docker image is tagged and pushed to:

**Amazon Elastic Container Registry (ECR)**

### 7. Automated EC2 Deployment

GitHub Actions uses **AWS Systems Manager (SSM)** to execute deployment commands on the EC2 instance.

The deployment:

1. Authenticates Docker with Amazon ECR.
2. Pulls the latest application image.
3. Stops the existing container.
4. Removes the existing container.
5. Starts a new container.
6. Exposes the Flask application on port `5000`.

## Security Implementation

Security is integrated throughout the pipeline.

| Security Layer       | Technology  | Purpose                                   |
| -------------------- | ----------- | ----------------------------------------- |
| Source code security | Bandit      | Static analysis of Python code            |
| Container security   | Trivy       | Detect vulnerabilities in Docker image    |
| AWS authentication   | GitHub OIDC | Short-lived AWS authentication            |
| AWS permissions      | IAM         | Control access to AWS resources           |
| Deployment access    | AWS SSM     | Remote deployment without GitHub SSH keys |
| Container isolation  | Docker      | Application containerization              |

## Technologies

### Application

* Python
* Flask
* Pytest

### DevOps

* Git
* GitHub
* GitHub Actions
* Docker

### Security

* Bandit
* Trivy
* AWS IAM
* GitHub OpenID Connect

### AWS

* Amazon EC2
* Amazon ECR
* AWS Systems Manager
* Amazon VPC

## Project Structure

```text
automated-devsecops-cicd/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── tests/
│   └── test_app.py
│
├── app.py
├── Dockerfile
├── requirements.txt
├── .gitignore
└── README.md
```

## Application Endpoints

### Home

```text
GET /
```

Returns:

```text
Automated DevSecOps CI/CD Pipeline is running!
```

### Health Check

```text
GET /health
```

Returns:

```json
{
  "status": "healthy"
}
```

## Deployment Flow

<img width="1214" height="1295" alt="image" src="https://github.com/user-attachments/assets/e07c981d-bf19-433e-9760-231b728cb1ce" />

## Screenshots

### GitHub Actions Workflow

The GitHub Actions workflow demonstrates the automated CI/CD pipeline, including testing, security scanning, Docker image building, and deployment stages.

![GitHub Actions Workflow](screenshots/github-actions-workflow.png)

### Successful ECR Deployment

The successful GitHub Actions workflow shows the Docker image being pushed to Amazon ECR.

![GitHub Actions ECR Success](screenshots/github-actions-ecr-success.png)

### Amazon ECR Repository

The Docker image is stored in a private Amazon ECR repository.

![Amazon ECR Repository](screenshots/ecr-repository.png)

### Docker Image in Amazon ECR

The ECR image details show the image tags, digest, repository, and active image status.

![Docker Image in ECR](screenshots/ecr-docker-image.png)

### EC2 Instance

The application was deployed to an Amazon EC2 instance running in the AWS Mumbai region.

![EC2 Instance Summary](screenshots/ec2-instance-summary.png)

### Running Docker Container

The `docker ps` output confirms that the deployed Flask application was running inside a Docker container on EC2 and exposing port `5000`.

![Running Docker Container](screenshots/ec2-docker-container.png)

### Deployed Application

The deployed Flask application was successfully accessed through the EC2 public IP address.

![Deployed Flask Application](screenshots/application.png)

### Health Check

The `/health` endpoint successfully returned a healthy status, confirming that the deployed application was responding correctly.

![Application Health Check](screenshots/health-check.png)


## Verification

The deployment was successfully verified through:

* Successful GitHub Actions workflow
* Successful Docker image push to Amazon ECR
* Successful AWS Systems Manager deployment
* Running Docker container on Amazon EC2
* Successful application response
* Successful `/health` endpoint response

The deployed Docker container exposes port:

```text
5000
```

## Key DevSecOps Concepts Demonstrated

This project demonstrates practical implementation of:

* Continuous Integration
* Continuous Deployment
* Static Application Security Testing (SAST)
* Container vulnerability scanning
* Docker containerization
* Container image registries
* AWS IAM
* GitHub OIDC authentication
* AWS Systems Manager
* Automated cloud deployment
* CI/CD security gates

## Future Improvements

Possible future improvements include:

* Deploying immutable Docker images using Git commit SHA tags
* Adding automated rollback functionality
* Adding application monitoring and logging
* Adding HTTPS with a domain name and load balancer
* Adding infrastructure provisioning using Terraform
* Adding automated dependency vulnerability scanning
* Adding deployment notifications

## Author

**Vyahruthi Goturu**

Cloud Computing Engineer | VIT Bhopal

GitHub: [VyahruthiG](https://github.com/VyahruthiG)
