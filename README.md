# Automated DevSecOps CI/CD Pipeline for AWS

## Overview

This project demonstrates an automated DevSecOps CI/CD pipeline for deploying a containerized application to AWS.

The pipeline will progressively automate:

- Application testing
- Source code security scanning
- Dependency vulnerability scanning
- Docker image building
- Container security scanning
- Docker image publishing to Amazon ECR
- Deployment to Amazon EC2

## Technologies

- Python
- Flask
- Pytest
- Git
- GitHub
- GitHub Actions
- Docker
- AWS EC2
- Amazon ECR
- AWS IAM
- AWS VPC

## Project Architecture

```text
Developer
    |
    v
  GitHub
    |
    v
GitHub Actions
    |
    +--> Tests
    |
    +--> Security Scans
    |
    +--> Docker Build
    |
    v
 Amazon ECR
    |
    v
 Amazon EC2
    |
    v
 Running Application