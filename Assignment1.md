# Assignment-01: Load Balancer & Auto Scaling Group (Spring 3 Hibernate Deployment)

The goal of this assignment is to **design and implement a cloud infrastructure** that supports the deployment of the [Spring 3 Hibernate](https://github.com/opstree/spring3hibernate.git) application. The infrastructure will prioritize **high availability, scalability, and security**. The application will be deployed on **private servers** to ensure a secure environment.

---

##  Application Repository

- **GitHub Link**: [https://github.com/opstree/spring3hibernate.git](https://github.com/opstree/spring3hibernate.git)

---



### 1. VPC Setup
- Create a custom VPC (e.g., CIDR: `10.0.0.0/16`)
- Create at least 2 **Public** and 2 **Private** subnets (across AZs)

<img width="1354" height="655" alt="img1" src="https://github.com/user-attachments/assets/6e226ac5-ed92-4b7e-bb1e-df7e8b632c9b" />



### 2. Internet & NAT Gateways
- Attach **Internet Gateway** to VPC
- Add **NAT Gateway** in a public subnet with an Elastic IP
- Configure routing tables:
  - Public Subnet → IGW
  - Private Subnet → NAT Gateway

### 3. Security Groups
- **ALB SG**:
  - Allow: HTTP (80), HTTPS (443) from 0.0.0.0/0
- **EC2 SG**:
  - Allow: HTTP from ALB SG only
  - Allow: SSH (optional - restrict by IP)
 
### Instances
-Public Server

<img width="1360" height="651" alt="img2" src="https://github.com/user-attachments/assets/0fbf06e5-844f-4718-a9fc-5cbd350a2347" />




### 4. Application Load Balancer (ALB)
- Launch ALB in public subnets
- Configure Listeners and Target Groups
- Attach Target Group to private EC2 instances

  <img width="1363" height="659" alt="img4" src="https://github.com/user-attachments/assets/b04c4939-90bc-4c9f-a5fc-55d861e4f786" />



### 5. Launch Template & Auto Scaling Group
- Create a Launch Template for EC2 with:
  - AMI (Amazon Linux or custom image with Spring app)
  - User data script to install dependencies and clone app
<img width="1363" height="647" alt="img3" src="https://github.com/user-attachments/assets/373ce66b-2e09-4a25-8730-c7954ba16e1d" />


    
- Create an Auto Scaling Group with:
  - Target Group association
  - Scaling policies (CPU > 60% → scale out, CPU < 30% → scale in)
  - Minimum 2 instances, maximum 5
 

  
<img width="1361" height="659" alt="img5" src="https://github.com/user-attachments/assets/740c062c-ce6c-47c7-92dd-0dd6fe088172" />


### 6. IAM Role
- Attach a role with EC2 permissions to access S3/CloudWatch (if needed)

---

##  Application Deployment

### Steps
1. Create AMI with Spring 3 Hibernate app pre-installed (or use user-data to install at runtime).
2. Ensure application runs on port 8080.
3. ALB Target Group must forward traffic to port 8080 on EC2.
4. Ensure security groups and routing are configured properly.
5. Verify application is accessible via ALB DNS.


---
<img width="1361" height="725" alt="img6" src="https://github.com/user-attachments/assets/8a6f8040-5891-48d7-a27f-99b15a053824" />


##  Validation

- Access application via the ALB DNS name.
- Trigger load to verify auto-scaling behavior.
- Check that EC2 instances in private subnets have outbound internet via NAT.
- Monitor scaling events and instance health in ASG and ALB target group.

---

##  Security Considerations

- EC2 instances are in **private subnets**, not accessible directly from the internet.
- ALB handles all incoming traffic and routes securely to EC2.
- NAT Gateway is used for secure outbound access from private subnets.
- IAM roles are used instead of hardcoded credentials.

---


---
## WEB/SITE VIEW

<img width="1363" height="723" alt="img7" src="https://github.com/user-attachments/assets/58919814-b9dd-4c2e-9ee1-cba677b0dd7a" />


---
