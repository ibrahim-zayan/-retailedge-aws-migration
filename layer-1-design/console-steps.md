# Layer 1 — Discovery & Architecture Design

## Task 1.1 — Architecture Diagram

Designed a Multi-AZ, three-tier AWS architecture: a public subnet
(ALB) per AZ, a private subnet (EC2 Auto Scaling) per AZ, and a
database subnet (RDS + ElastiCache) per AZ, fronted by Route 53 and
CloudFront, with S3 for static assets and NAT Gateways for outbound
access from the private tier.

![Architecture Diagram](./architecture-diagram.png)

## Task 1.2 — Migration Strategy

| Component   | Decision          | Reason                                                              |
|-------------|-------------------|----------------------------------------------------------------------|
| Web Server  | Rehost            | Code stays as-is; improvement is in infrastructure (Auto Scaling), not code |
| Application | Replatform        | 90-day deadline; rewriting risks breaking functionality for 200K users |
| Database    | Replatform (RDS)  | AWS manages backup, patching, and automatic Multi-AZ failover        |

## Task 1.3 — TCO Comparison (3-Year Projection)

| Service | Monthly Cost |
|---|---|
| Amazon EC2 | $100.34 |
| Amazon RDS for MySQL | $122.28 |
| Amazon ElastiCache | $46.72 |
| VPC (NAT Gateway) | $74.70 |
| Elastic Load Balancing | $28.11 |
| Amazon S3 | $1.24 |
| Amazon CloudFront | $17.70 |
| Amazon Route 53 | $0.90 |
| **Total (monthly)** | **$391.99** |
| **Total (annual)** | **≈ $4,704** |

| | Current (On-Premises) | AWS |
|---|---|---|
| Annual cost | $18,000 | ≈ $4,704 |
| 3-year cost | $54,000 | ≈ $14,112 |

**Estimated 3-year savings: ~$40,000 (~74% reduction)**

> Note: this comparison covers infrastructure costs only. It does
> not include one-time migration effort (team hours, testing, training).
