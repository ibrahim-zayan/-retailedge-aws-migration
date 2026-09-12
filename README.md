# RetailEdge — AWS Migration Project

Solutions Architect simulation: migrating a mid-size e-commerce company
from 3 bare-metal servers to a Multi-AZ, Three-Tier AWS architecture.

## Layer 1 — Architecture Design

### Architecture Diagram

### Migration Strategy

| Component   | Decision          | Reason                                                                      |
|-------------|-------------------|-----------------------------------------------------------------------------|
| Web Server  | Rehost            | Code stays as-is; improvement is in infrastructure (Auto Scaling), not code |
| Application | Replatform        | 90-day deadline; rewriting risks breaking functionality for 200K users      |
| Database    | Replatform (RDS)  | AWS manages backup, patching, and automatic Multi-AZ failover               |
### TCO Comparison (3-Year Projection)

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

> Note: this comparison covers infrastructure costs only. It does not
> include one-time migration effort (team hours, testing, training).
