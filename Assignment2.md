# Assignment-02: Deployment Strategies with Amazon S3 Integration

##  Objective

The goal of this assignment is to explore various **deployment strategies** and implement them while integrating **Amazon S3** for managing static assets, configuration files, and deployment artifacts. This ensures better scalability, easier version management, and decoupled asset storage for applications.

---

##  Deployment Strategies: Concepts, Pros & Cons

### 1.  Recreate Deployment

**Description**:  
In the recreate deployment strategy, the existing application version is stopped entirely before deploying the new version. This results in **downtime** while the old version is shut down and the new one starts.

**Advantages**:
- Simple and easy to implement.
- No need for complex traffic switching logic.

**Disadvantages**:
- Causes downtime.
- Risky if the new version has issues — rollback is manual and time-consuming.

---
### EC2 Instance
<img width="1363" height="651" alt="img1" src="https://github.com/user-attachments/assets/3ffd4ec9-3cf4-4b7a-9ad7-f2743df47ac0" />


### S3 bucket

<img width="1361" height="653" alt="img2" src="https://github.com/user-attachments/assets/1f0e9ebf-bdc6-425b-8c70-6154653dde45" />


### index.html

<img width="1351" height="717" alt="img3" src="https://github.com/user-attachments/assets/639ad489-4b19-43b0-9702-9e9e77fae2d8" />


### Website Deployment Version 1

<img width="1357" height="677" alt="img4" src="https://github.com/user-attachments/assets/894d4fb5-60c9-404a-b90b-3d8195ec436f" />

### AMI template

<img width="1357" height="651" alt="img6" src="https://github.com/user-attachments/assets/c1060de7-43ee-47eb-8c90-46992f3c5ece" />



## 2.  Rolling Deployment

**Description**:  
In rolling deployment, instances are gradually replaced with new versions. The update happens **one or few instances at a time** to ensure partial availability during deployment.

**Advantages**:
- Minimizes downtime.
- Supports easy rollback if only a few instances are updated.

**Disadvantages**:
- Complex version management across instances.
- Partial user exposure to new versions.

---
### S3 bucket object (style.css , logo.png)
<img width="1599" height="601" alt="img1" src="https://github.com/user-attachments/assets/d84fa739-6240-4c96-938e-e2481e766921" />


### Ec2 Instances
<img width="1599" height="601" alt="img1" src="https://github.com/user-attachments/assets/32d6989c-f2ce-479b-abad-8ef90bb9d603" />


### Auto Scaling Group
<img width="1585" height="651" alt="img3" src="https://github.com/user-attachments/assets/5dcdd29b-07dd-419b-856e-6dd01b407e7a" />


### Launch Template
<img width="1589" height="343" alt="img4" src="https://github.com/user-attachments/assets/aa74dd3a-931d-4307-94a5-b50fc1a75e25" />


### AMI images
<img width="1589" height="343" alt="img4" src="https://github.com/user-attachments/assets/8bc9c1d7-0722-4fef-bc53-60258c5cf0cd" />


### Vesrion 1
<img width="1396" height="511" alt="img6" src="https://github.com/user-attachments/assets/7276cd61-c73e-4f2b-99a4-39b700089d9c" />


### version 2
<img width="1139" height="520" alt="img7" src="https://github.com/user-attachments/assets/fd7c6e07-25f2-4cbb-8175-572a0d99e5f6" />




### 3.  Blue-Green Deployment

**Description**:  
This strategy maintains two identical environments — **blue (live)** and **green (new version)**. Traffic is switched to the green environment once validation is complete.

**Advantages**:
- No downtime if switch is instant.
- Easy rollback by reverting traffic to the blue environment.

**Disadvantages**:
- Requires duplicate infrastructure.
- Higher cost due to dual environments.

---

### 4.  A/B Deployment

**Description**:  
A/B deployment sends traffic to two or more different versions of the application (A and B), mainly to compare user engagement or performance metrics.

**Advantages**:
- Ideal for feature testing and experimentation.
- Controlled exposure and testing.

**Disadvantages**:
- Complex routing setup.
- Might confuse users if not well managed.

---

### 5.  Canary Deployment

**Description**:  
Canary deployment releases the new version to a **small subset of users/instances** first. If no issues are found, the deployment is gradually expanded.

**Advantages**:
- Minimizes risk.
- Can detect issues early and prevent full rollout.

**Disadvantages**:
- Requires monitoring and traffic splitting.
- Slightly more complex infrastructure.

---

##  Implementation Tasks

###  Recreate Deployment (Must Do)

- **Environment**: Single EC2 Instance
- **Steps**:
  - Create an EC2 instance and install the application.
  - Generate an **AMI snapshot** for backup and versioning.
  - Shut down the old version, launch a new EC2 using the AMI.
  - Store static assets (e.g., CSS, JS, images) in an **Amazon S3 bucket**.
  - Configure the app to fetch assets directly from S3.

---

###  Rolling Deployment (Must Do)

- **Environment**: Auto Scaling Group (ASG)
- **Steps**:
  - Create ASG with initial version using **Launch Template A**.
  - Store deployment artifacts (e.g., zipped app, AMI details) in S3.
  - Create a new Launch Template (or configuration) with updated version.
  - Update the ASG to use the new version gradually.
  - Monitor instance replacement and health.

---

###  Blue-Green Deployment (Good To Do)

- **Environment**: Two EC2 Environments (Blue & Green)
- **Steps**:
  - Deploy "Blue" environment with live application.
  - Create "Green" environment with the updated version.
  - Store environment configs and assets in S3.
  - Use **Application Load Balancer (ALB)** to switch traffic to "Green".
  - Revert to Blue if issues are detected.

---

###  Canary Deployment (Good To Do)

- **Environment**: ASG with Weighted Instances
- **Steps**:
  - Deploy new version to 1 or 2 instances (canaries).
  - Use ALB Target Groups to send a small percentage of traffic to canaries.
  - Monitor logs, metrics, and user feedback (store logs in S3).
  - Gradually add more instances if stable.

---


