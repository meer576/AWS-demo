## Assignment-03: NGINX Reverse Proxy with HA/DR, S3 Integration, and Path-Based Routing
###Objective
To set up a highly available and disaster recovery-enabled NGINX reverse proxy using EC2, Auto Scaling Groups (ASG), Load Balancer, and Amazon S3 for hosting static content. The solution must also handle versioning, rolling and blue-green deployments, IAM-based access control, and CDN integration for optimized latency.

## Day-Wise Task Breakdown
## Day 1 – NGINX Setup with Versioning and Auto Scaling

### Tasks:
- Launch EC2 and install NGINX, then create AMI-1.
- Launch new EC2 instance using AMI-1 and label it as V1.
- Modify NGINX (e.g., update version/config), then create AMI-2.
- Launch another EC2 from AMI-2 and label it as V2.
- You now have two versions of the AMI: V1 and V2.

### High Availability Setup:
- Create a Launch Template with AMI.
- Create an Auto Scaling Group (ASG) using this template.
- Attach ASG to an Application Load Balancer (ALB).
- Configure scaling policies:
  - Average CPU Utilization
  - Network In/Out
  - ALB Request Count per Target
### Rolling Deployment:
- Update ASG to use AMI-2 (V2) for upgrade.
- Client reports incompatibility → Revert ASG to use AMI-1 (V1).
  
<img width="1599" height="601" alt="img1" src="https://github.com/user-attachments/assets/ba836aec-e527-4303-9879-b42ab126fccb" />

<img width="1593" height="649" alt="img2" src="https://github.com/user-attachments/assets/1b1eb910-b6ef-42e8-adbe-b535a11444e7" />

<img width="1585" height="651" alt="img3" src="https://github.com/user-attachments/assets/71fe14b8-f461-42d3-99a4-ef10d80381df" />

<img width="1589" height="343" alt="img4" src="https://github.com/user-attachments/assets/aa10b282-e8a1-44ba-890c-1cb5ec43665b" />

<img width="1593" height="361" alt="img5" src="https://github.com/user-attachments/assets/4b7ff27a-08f5-4718-8917-e30f2051c54e" />

<img width="1396" height="511" alt="img6" src="https://github.com/user-attachments/assets/4cf187fe-8afc-46f4-b949-8dd8d2ae19e4" />

<img width="1139" height="520" alt="img7" src="https://github.com/user-attachments/assets/83dae5e8-bf7c-426a-8c8c-5cfb3b2d1129" />

## Day 2 – Web Hosting with S3 & Git Integration via EC2 Only

### Tasks:
- Clone image files from a VCS Git repository using EC2.
- Push these images to Amazon S3 using aws-cli configured via IAM role (no access keys).
- Modify NGINX index page to fetch images from S3 Bucket URL.
- Ensure all operations are performed via EC2 only, as per client policy.

---

## Day 3 – Unhealthy Server Simulation and ASG Validation

### Tasks:
- SSH into NGINX EC2 instance and simulate failure (e.g., stop nginx, corrupt file).
- Monitor Auto Scaling Group behavior:
  - ASG should detect unhealthy instance and replace it automatically.
- Logs and alerts must verify instance replacement and health checks.

---

## Day 4 – Path-Based Routing Using ALB

### Infrastructure Design:
- 1 Bastion Host in public subnet (SSH access only from your IP).
- 2 NGINX Servers in private subnets, accessible via Bastion.
- Each NGINX server serves a unique welcome page:
  - `/ninja1 → Image-1`
  - `/ninja2 → Image-2`

### ALB Configuration:
- ALB listens on port 80, accepts traffic only from your IP.
- Create 2 target groups, one for each EC2 instance.
- Add listener rules:
  - `/ninja1` routes to first EC2
  - `/ninja2` routes to second EC2
- Update NGINX pages to load respective images from S3.

---

## Day 5 – S3 Bucket Policies and IAM Restrictions

### Tasks:
- Create S3 Bucket in `us-east-1`.
- Add two folders: `prod/` and `nonprod/`.
- Upload different images to each folder.

### IAM Setup:
- Create an IAM Role with full S3 access (used by NGINX app/EC2).
- Create an IAM User:
  - Can access `nonprod/` folder only.
  - Deny access to `prod/` folder explicitly.

### Access Control:
- IAM and Bucket Policies:
  - Bucket accessible to root, IAM user (restricted), and EC2 instance.
- Use resource-based policies and IAM conditions.

---

## Day 6 – CloudFront & IAM Trust Relationship

### Tasks:
- Use CloudFront (CDN) to serve images stored in S3.
- Use same IAM role from EC2 to grant CDN access (no policy changes).

### Validation:
- Establish a trust between CloudFront and IAM Role.
- Create a new IAM user with minimal permissions.

---

## GOOD TO DO – Full Automation (Bonus)

### Blue-Green Deployment:
- Maintain two environments: Blue (live), Green (staging).
- ALB switches traffic between environments.
- Perform zero-downtime upgrades and fast rollbacks.

### CloudFront Optimization:
- Place CloudFront in front of ALB to reduce latency.
- Cache static resources from S3 for performance.

### Rolling Deployment Automation:
- Create utility to:
  - Select AMI version
  - Attach AMI to ASG
  - Trigger rolling deployment
  - Revert back to previous version if needed
