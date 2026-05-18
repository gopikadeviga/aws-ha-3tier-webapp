# Cost Analysis — AWS HA 3-Tier Architecture

## Monthly Cost Estimate (us-east-1)

### Compute
| Resource | Spec | Cost/month |
|----------|------|-----------|
| EC2 x2 | t2.micro | ~$17 |
| ALB | 1 instance | ~$16 |
| NAT Gateway x2 | per AZ | ~$65 |

### Database
| Resource | Spec | Cost/month |
|----------|------|-----------|
| RDS Multi-AZ | db.t3.micro | ~$25 |
| RDS Storage | 20 GiB gp3 | ~$2.30 |

### Storage & CDN
| Resource | Spec | Cost/month |
|----------|------|-----------|
| S3 | 1 GB storage | ~$0.02 |
| CloudFront | 1 TB transfer | ~$8.50 |

### Monitoring
| Resource | Spec | Cost/month |
|----------|------|-----------|
| CloudWatch | 4 alarms | ~$0.40 |
| VPC Flow Logs | 5 GB/month | ~$0.25 |
| AWS Config | per resource | ~$2 |
| AWS Backup | RDS snapshots | ~$0.10 |

## Total Estimated: ~$136/month

## Cost Optimisation Opportunities
- Use Reserved Instances for EC2 (save 40-60%)
- Use Savings Plans for predictable workloads
- Single NAT Gateway in dev/test (save $32/month)
- CloudFront price class: US/EU only (already done)
- RDS automated storage scaling prevents over-provisioning
