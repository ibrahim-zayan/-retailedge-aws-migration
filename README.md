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
