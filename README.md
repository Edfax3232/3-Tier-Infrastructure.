## **3-Tier-INFRASTRUCTURE-DEPLOYMENT**
This is a project combining the 3-Tier Web-Tier, Application-Tier and Database-Tier.

This project involves the deployment of a highly available, secure, and scalable three-tier web application infrastructure in Amazon Web Services (AWS). The architecture separates the application into three logical tiers:

1.## **WEB TIER**
Hosted in ECS containers within public subnets.

Load-balanced using Application Load Balancer (ALB).

Internet access enabled via CloudFront, Route 53, and an Internet Gateway.

Frontend containerized services reside in ECS and are protected by security groups and routing rules.

2. ## **APPLICATION TIER**
Deployed in ECS within private subnets (protected from the internet).

Communicates securely with the web tier using security groups.

Uses Auto Scaling for elasticity and handles business logic.

Integration with AWS Fargate and Task Definitions managed through IAM roles.

3. ## **DATABASE TIER**
Includes Amazon RDS for MySQL and Amazon FSx for persistent storage.

Hosted in private subnets with no direct internet access.

Integrated with AWS DataSync for data movement and backup.

Security and Monitoring
Each tier is protected by dedicated security groups and route tables.

IAM roles are used for secure service-to-service interactions.

Logging and monitoring via CloudWatch.

DNS resolution and domain routing handled via Route 53.

⚠️ ## **CHALLENGES AND SOLUTION**
1. **Challenge: Container Networking Between Tiers**
Problem: Establishing secure and performant communication between ECS services in separate subnets.

Solution: Used security group references and private subnets with custom route tables to isolate traffic while enabling cross-tier connectivity.

2. **Challenge: Database Restore & High Availability**
Problem: MySQL in RDS required multi-AZ setup and data migration without downtime.

Solution: Enabled Multi-AZ RDS deployment, used AWS DataSync to replicate data, and scripted failover testing for disaster recovery validation.

3. **Challenge: IAM Role Misconfiguration**
Problem: ECS tasks failed due to incorrect IAM permissions for task execution and service discovery.

Solution: Created fine-grained IAM roles and policies for ECS tasks and services, allowing only the required access.

4. **Challenge: Auto Scaling of Services**
Problem: Application tier services were not scaling effectively under load.

Solution: Tuned CloudWatch alarms and ECS service auto-scaling policies, using CPU and memory utilization metrics.

5. **Challenge: Secure Public Access**
Problem: Users needed public access to the frontend but all backend systems had to remain private.

Solution: Implemented CloudFront with Route 53 DNS, WAF rules, and used ALB to restrict access to only the web tier while denying direct access to the application and database tiers.

6. **Challenge: Deployment Automation**
Problem: Manual deployment was error-prone and time-consuming.

Solution: Introduced Terraform for infrastructure as code (IaC), enabling version-controlled, repeatable deployments.

🧩 ## **Optional Enhancements (if not already implemented)**
Use of Secrets Manager or Parameter Store for secure configuration.

Integration with CloudTrail for auditing and compliance.

Set up CI/CD pipelines using CodePipeline or GitHub Actions for zero-downtime deployments.


