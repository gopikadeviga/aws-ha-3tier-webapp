# AWS-HA-3Tier-WebApp
Cloud-Native Highly Available 3-Tier  Web Application - AWS

## LIVE DEMO
   Portfolio : https://d3d6c6f0oluiq.cloudfront.net
   
## ARCHITECTURE OVERVIEW

   3-tier web application deployed on AWS, spanning 2 Availability Zones with auto-healing compute,
   managed database, and global CDN delivery.

## ARCHITECTURAL DIAGRAM

<img width="931" height="826" alt="final-architecture-diagram" src="https://github.com/user-attachments/assets/6a4f9a92-3b2c-4989-bf95-68af04035c6b" />


## VPC ARCHITECTURE DIAGRAM

<img width="1579" height="671" alt="01-vpc-architecture" src="https://github.com/user-attachments/assets/371f6c58-0590-4bd0-b51d-88e713ffb0ec" />

## AWS SERVICES USED

| Layer | Service | Purpose |
|-------|---------|---------|
| Network | VPC, Subnets, IGW, NAT | Isolated network |
| Compute | EC2, Auto Scaling, ALB | HA app servers |
| Database | RDS MySQL | Managed database |
| CDN | CloudFront, S3 | Global static delivery |
| Monitor | CloudWatch, SNS | Alerts + observability |
| Audit | VPC Flow Logs, Config | Security + compliance |
| Backup | AWS Backup | Automated DR(tested) |

## KEY DESIGN DECISIONS

- One NAT Gateway per AZ eliminates cross-AZ SPOF
- ELB health checks on ASG detect app-level failures
- CloudFront with OAC — private S3 access without
  making bucket public
- Security group chaining: ALB → EC2 → RDS only
- Isolated DB subnets — RDS has zero internet route

## INFRASTRUCTURE DETAILS

VPC CIDR: 10.0.0.0/16 across us-east-1a + us-east-1b
Subnets: 2 public + 2 private + 2 DB (6 total)
EC2: t3.micro, Amazon Linux 2023, ASG min2/max4
RDS: MySQL 8.0, db.t3.micro, automated failover
ALB: Internet-facing, multi-AZ, ELB health checks
CloudFront: Dual-origin (S3 static + ALB dynamic)

## SCREENSHOTS

### Live Portfolio on CloudFront

<img width="924" height="1017" alt="02-protfolio-live" src="https://github.com/user-attachments/assets/30de16fa-64ef-4306-a5d8-d83d443964fa" />

### Target Group — Healthy Instances

<img width="1369" height="660" alt="03-target-group" src="https://github.com/user-attachments/assets/883d9144-7b29-40a9-94db-a44feb6f7e1d" />

### Auto Scaling Group Activity

<img width="1201" height="788" alt="04-asg-activity" src="https://github.com/user-attachments/assets/7950942a-3d89-4db8-a514-6750a942d66d" />

### CloudWatch Alarms Dashboard

<img width="790" height="339" alt="05-cloudwatch-alarms" src="https://github.com/user-attachments/assets/248ea497-d2e5-4f57-b28e-d0b9f556cc3b" />

### VPC Flow Logs — Active Streams

<img width="1066" height="419" alt="06-vpc-flow-logs" src="https://github.com/user-attachments/assets/2caf40c0-ef67-4272-802b-dc0d1589cfee" />

### RDS Database 

<img width="1060" height="420" alt="07-rds-database-c s" src="https://github.com/user-attachments/assets/a4f2000c-cae3-42b1-8259-de07c2c07bcc" />

### RDS Database Configuration

<img width="1032" height="609" alt="07-rds-database-config" src="https://github.com/user-attachments/assets/9efd4c57-1798-4ce8-a060-e7e5ef63168d" />

## RDS Multi-AZ Upgrade & Failover Test

### RDS Database After Multi-AZ enabled

<img width="1586" height="772" alt="07 2-rds-after-multiaz" src="https://github.com/user-attachments/assets/24dd77ca-479b-4e2d-8945-e65cb0d5512d" />

### RDS Before Failover

<img width="659" height="762" alt="07 3-rds-before-failover" src="https://github.com/user-attachments/assets/090f8db1-9295-4609-b1a8-2819c4a14888" />

### RDS rebooting With Failover

<img width="668" height="237" alt="07 4-rds-rebooting-with-failover" src="https://github.com/user-attachments/assets/4a11c8de-97bc-4257-8e3a-21340cdc0ff6" />

- Upgraded to Multi-AZ and tested automatic failover by rebooting with failover enabled.
  
### RDS Primary After Failover 

<img width="677" height="767" alt="07 5-rds-primary-after-failover" src="https://github.com/user-attachments/assets/9f57ecb0-d4ce-4d27-99be-daa6ab285228" />

- RDS MySQL 8.0 — Multi-AZ tested with automatic failover. 
- Primary AZ switched us-east-1a → us-east-1bin under 2 minutes. 
- RTO < 2 min, RPO = 0.

### Application Load Balancer

<img width="1503" height="322" alt="08-alb-details" src="https://github.com/user-attachments/assets/28c79f68-6a00-484f-bd29-7ff1bea4d7cf" />

## WHAT I LEARNED :

- Designing HA architecture across multiple AZs
- Difference between ELB and EC2 health checks
- CloudFront dual-origin routing with OAC
- Security group chaining for least privilege
- CloudWatch alarm configuration and SNS integration
