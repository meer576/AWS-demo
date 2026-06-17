# Assignment 05: Modular Infrastructure Implementation using Terraform 

##  Objective

The goal of this assignment is to implement AWS infrastructure using **Terraform Modules** for scalability, reusability, and maintainability. This includes using **Amazon S3** for remote Terraform state management and **DynamoDB** for state locking to prevent concurrent modifications.

---

##  Project Structure

The Terraform code is structured into reusable modules to separate the logic of each major component. A root module ties everything together.

###  Directory Structure

```bash
.
├── compute
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
├── networking
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
├── root
│   ├── main.tf
│   ├── outputs.tf
│   └── variables.tf
└── security
    ├── main.tf
    ├── outputs.tf
    └── variables.tf
````

---

##  Modules

### 1. `vpc/`

Creates a Virtual Private Cloud (VPC) with CIDR block and default tags.

### 2. `subnet/`

Provisions public and private subnets across availability zones.

### 3. `security-group/`

Creates required security groups for EC2, ALB, SSH, and HTTP/HTTPS access.


<img width="1365" height="659" alt="img2" src="https://github.com/user-attachments/assets/3ad72178-06db-40c5-9fcd-46092191d187" />


### 4. `ec2/`

Launches EC2 instances within specified subnets and attaches appropriate security groups.


<img width="1363" height="685" alt="img3" src="https://github.com/user-attachments/assets/fc218b63-8364-41d7-bfff-df040674336f" />

---

##  Root Module

The root module integrates all child modules by passing variables and outputting the essential infrastructure components.

###  Example Usage (main.tf)

```hcl
module "vpc" {
  source = "./modules/vpc"
  cidr_block = var.vpc_cidr
  ...
}

module "public_subnets" {
  source = "./modules/subnet"
  ...
}

module "ec2_instance" {
  source = "./modules/ec2"
  ...
}
```

---

##  Terraform Remote State Management (S3)

To avoid local state files and enable collaboration, the Terraform state file is stored in an **S3 bucket**.

###  State Locking with DynamoDB

State locking is implemented using a DynamoDB table to avoid concurrent runs and corruption.

###  Backend Configuration (`backend.tf`)

```hcl
terraform {
  backend "s3" {
    bucket         = "your-tfstate-bucket"
    key            = "env/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-lock-table"
    encrypt        = true
  }
}
```

---

##  Provider Version Locking

Pinned provider versions ensure repeatability and avoid unexpected behavior due to provider updates.

###  Example (`provider.tf`)

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  required_version = ">= 1.3.0"
}

provider "aws" {
  region = var.aws_region
}
```

---

##  Execution Steps

###  Initialize Terraform

```bash
terraform init
```

###  Validate Configuration

```bash
terraform validate
```

###  Show Execution Plan

```bash
terraform plan
```

###  Apply Infrastructure

```bash
terraform apply
```



###  Destroy Infrastructure

```bash
terraform destroy
```

---

##  Best Practices Followed

*  Modular architecture for reusability
*  Remote backend configuration with S3
*  State locking using DynamoDB
*  Version-controlled Terraform provider
*  Clean and maintainable variable naming
*  Secure IAM usage and least-privilege policies
*  Proper tagging for resource management

---

##  Notes

* Ensure **S3 bucket** and **DynamoDB table** exist before running Terraform.
* Keep your `terraform.tfvars` file secure and untracked using `.gitignore`.
* Use environment-specific directories (e.g., `dev`, `prod`) for separate deployments.
* Backup your `.tfstate` files and rotate credentials regularly.

---
