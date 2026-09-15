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

---

## Layer 2 — Network Foundation & Security

Built a VPC (10.0.0.0/16) with 6 subnets across 2 Availability Zones,
an Internet Gateway, 2 NAT Gateways, and 4 route tables enforcing
strict tier isolation (public, private, database). Created 3
chained security groups (alb-sg → app-sg → rds-sg) so each tier only
accepts traffic from the tier directly in front of it.

Full details: [layer-2-network/console-steps.md](./layer-2-network/console-steps.md)

## Layer 3 — Compute & Auto Scaling

Built a Launch Template, Application Load Balancer, Target Group,
and Auto Scaling Group (min=2, max=10) serving a PHP application on
port 8080 behind the ALB on port 80. Configured a target tracking
scaling policy (60% CPU) and a scheduled scaling action for Friday
evenings, then verified the site serves traffic correctly through
the ALB.

Full details: [layer-3-compute/console-steps.md](./layer-3-compute/console-steps.md)

## Layer 4 — Data Layer & Migration

Provisioned RDS MySQL Multi-AZ (`retailedge-db`) with encrypted
storage and 7-day backups, plus an ElastiCache Redis cluster
(`retailedge-cache`) with Multi-AZ and encryption enabled. Wrote a
4-phase migration cutover plan (Full Load, CDC, Cutover, Rollback)
achieving near-zero RPO and under 2-minute RTO.

Full details: [layer-4-data/console-steps.md](./layer-4-data/console-steps.md)
| [Migration Plan](./layer-4-data/migration_plan.md)
