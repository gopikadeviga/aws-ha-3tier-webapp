# ARCHITECTURE DESIGN NOTES

## NETWORK DESIGN
- VPC CIDR 10.0.0.0/16 chosen for max flexibility
- Separate DB subnets (not private) so RDS has
  zero internet route — not even through NAT
- One NAT Gateway per AZ to eliminate cross-AZ
  single point of failure

## Compute Design
- ELB health checks on ASG instead of EC2 checks
  so app-level failures trigger replacement
- Desired: 2 instances minimum across 2 AZs
  so one AZ failure still serves traffic
- t2.micro for cost efficiency in lab environment

## Database Design
- DB Subnet Group spans both AZs
- sg-rds only accepts traffic from sg-app
  using security group reference (not IP)
- No public access — only reachable from
  EC2 instances inside the VPC

## CDN Design
- CloudFront with dual-origin routing:
  /static/* → S3 (cached, CachingOptimized)
  /* → ALB (not cached, CachingDisabled)
- OAC instead of OAI for S3 access
  (more secure, supports all regions)
- HTTP → HTTPS redirect enforced at edge

  ## Security Design
- Security group chaining:
  Internet → ALB (sg-alb)
  ALB → EC2 (sg-app, source: sg-alb)
  EC2 → RDS (sg-rds, source: sg-app)
- S3 VPC Endpoint for private S3 access
  without traversing public internet
- Block Public Access ON for S3 bucket

## Monitoring Design
- 4 CloudWatch Alarms covering:
  ALB errors, EC2 CPU, RDS CPU, RDS storage
- VPC Flow Logs capturing ALL traffic (accept+reject)
- AWS Config recording all resource changes
- AWS Backup daily at 03:00 UTC, 30-day retention
