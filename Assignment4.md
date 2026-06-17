
# Assignment-04: Terraform Infrastructure as Code (IaC) - MongoDB

## Objective

The objective of this assignment is to design and implement a MongoDB infrastructure on AWS using Terraform following Infrastructure as Code (IaC) best practices.

---

# Phase 1 - Infrastructure Architecture Design

## Infrastructure Architecture

The infrastructure consists of the following AWS components:

- VPC
- Public Subnet
- Private Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Network ACLs
- Bastion Host
- MongoDB Primary Instance
- MongoDB Secondary Instance
- Amazon S3 Bucket
- DynamoDB Table
- IAM Roles

### Architecture Diagram

<img width="1192" height="1263" alt="final-mongo-diagram" src="https://github.com/user-attachments/assets/c6619fa5-fb5a-4bbc-a24c-0ca90c6e543d" />


---

# Phase 2 - Terraform Implementation

## Project Structure

```text
.
├── compute
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
│
├── networking
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
│
└── security
    ├── main.tf
    ├── outputs.tf
    └── variables.tf
````

## Terraform Modules

### Networking Module

Responsible for:

* VPC Creation
* Public & Private Subnets
* Internet Gateway
* NAT Gateway
* Route Tables

### Security Module

Responsible for:

* Security Groups
* NACLs
* IAM Roles

### Compute Module

Responsible for:

* Bastion Host
* MongoDB Primary Server
* MongoDB Secondary Server

---

# Remote Terraform State Management

## Why Remote State?

Remote state provides:

* Team Collaboration
* Centralized State Storage
* State Consistency
* CI/CD Integration
* State Locking

---

## S3 Backend Configuration

Terraform state file is stored in an S3 bucket.

```hcl
terraform {
  backend "s3" {
    bucket         = "mongodb-terraform-state"
    key            = "terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

---

## DynamoDB State Locking

DynamoDB is used for state locking to prevent multiple users from modifying the infrastructure simultaneously.

---

# Deployment Steps

## Initialize Terraform

```bash
terraform init
```

## Validate Terraform Configuration

```bash
terraform validate
```

## Review Execution Plan

```bash
terraform plan
```

## Apply Infrastructure

```bash
terraform apply
```

## Destroy Infrastructure

```bash
terraform destroy
```

---

# Terraform Apply Output

Terraform successfully provisioned the required AWS resources.

<img width="1358" height="728" alt="img1" src="https://github.com/user-attachments/assets/007a4f0f-91b7-4161-b64b-f2ad4a4c0c29" />


---

# VPC Network Map

The networking layer consists of:

* Custom VPC
* Public Subnet
* Private Subnets
* Internet Gateway
* NAT Gateway
* Route Tables

<img width="1363" height="691" alt="img2" src="https://github.com/user-attachments/assets/40660719-ef46-495c-bfdd-10da72fc47a2" />


---

# EC2 Instances

The following EC2 instances were deployed:

* Bastion Host
* MongoDB Primary Instance
* MongoDB Secondary Instance

<img width="1359" height="653" alt="img3" src="https://github.com/user-attachments/assets/c1ed6dbe-99d4-426f-8330-e6339cca9365" />


---

# Amazon S3 Bucket

The S3 bucket is used for:

* Remote Terraform State Storage
* State Versioning
* Secure State Management

<img width="1363" height="659" alt="img4" src="https://github.com/user-attachments/assets/99991183-0413-44a3-a346-9d94b32a4b0a" />


---

# DynamoDB Table

The DynamoDB table is used for:

* Terraform State Locking
* Preventing Concurrent Operations

<img width="1363" height="655" alt="img5" src="https://github.com/user-attachments/assets/67f67af7-04be-45fb-97b0-ffcc40e09e34" />


---

# AWS Resources Created

* VPC
* Public Subnet
* Private Subnets
* Internet Gateway
* NAT Gateway
* Route Tables
* Security Groups
* NACLs
* Bastion Host
* MongoDB Primary EC2
* MongoDB Secondary EC2
* S3 Bucket
* DynamoDB Table
* IAM Roles

---

# Best Practices Implemented

* Infrastructure as Code (IaC)
* Modular Terraform Design
* Remote State Storage
* State Locking
* Resource Tagging
* Reusable Modules
* Secure Network Architecture
* Separation of Concerns

---

# Technologies Used

* Terraform
* AWS EC2
* AWS VPC
* AWS S3
* AWS DynamoDB
* AWS IAM
* MongoDB

---

# Author

**Khizar Meer**

DevOps Training Assignment - Terraform Infrastructure as Code (IaC)

```
```
