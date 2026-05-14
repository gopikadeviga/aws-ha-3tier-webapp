# aws-ha-3tier-webapp
Cloud-Native Highly Available 3-Tier  Web Application - AWS

## LIVE DEMO
>> Portfolio : https://d3d6c6f0oluiq.cloudfront.net

## ARCHITECTURE OVERVIEW
>> 3-tier web application deployed on AWS, spanning 2 Availability Zones with auto-healing compute,
   managed database, and global CDN delivery.

## ARCHITECTURE DIAGRAM


## AWS SERVICES USED

| Layer | Service | Purpose |
|-------|---------|---------|
| Network | VPC, Subnets, IGW, NAT | Isolated network |
| Compute | EC2, Auto Scaling, ALB | HA app servers |
| Database | RDS MySQL | Managed database |
| CDN | CloudFront, S3 | Global static delivery |
| Monitor | CloudWatch, SNS | Alerts + observability |
| Audit | VPC Flow Logs, Config | Security + compliance |
| Backup | AWS Backup | Automated DR |

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
RDS: MySQL 8.0, db.t3.micro, automated backups
ALB: Internet-facing, multi-AZ, ELB health checks
CloudFront: Dual-origin (S3 static + ALB dynamic)

## WHAT I LEARNED

- Designing HA architecture across multiple AZs
- Difference between ELB and EC2 health checks
- CloudFront dual-origin routing with OAC
- Security group chaining for least privilege
- CloudWatch alarm configuration and SNS integration
