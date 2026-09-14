# Layer 3 — Compute & Auto Scaling (Console Steps)

## Task 3.1 — Launch Template + Auto Scaling Group

Created a launch template (`retailedge-launch-template`, t3.micro,
Amazon Linux 2023) with user data installing Apache + PHP on port 8080.
Created an Auto Scaling Group (`retailedge-asg`, min=2, max=10,
desired=2) across the private subnets, attached to an ALB
(`retailedge-alb`) and target group (`app-target-group`) listening
on port 80 and forwarding to port 8080.

![Launch Template](./launch-template.png)
![Target Group Healthy](./target-group-healthy.png)
![Server Running](./server-running.png)

## Task 3.2 — Scaling Policy

Created a target tracking scaling policy (`cpu-target-tracking`) on
Average CPU Utilization with a target value of 60%, allowing both
scale-out and scale-in.

## Task 3.3 — Answers

**What happens when CPU hits 60% on one instance, step by step?**

CloudWatch continuously monitors the average CPU utilization across
all running instances. Once the average stays above 60% for a
sustained period (not just a single spike), the target tracking
policy triggers the ASG to launch a new instance from the same
launch template.

**When does a new instance start receiving traffic, and why not
immediately?**

Only after the instance finishes its user data script (installing
Apache/PHP and starting the service) and passes the ALB's health
check during the health check grace period. Only then does the ALB
mark it Healthy and start routing traffic to it.

**Why did we choose min=2 instead of min=1?**

To guarantee availability across both Availability Zones. With
min=1, the single running instance could be in the AZ that fails,
taking the whole application down. With min=2 (one per AZ), the
other AZ keeps serving traffic if one AZ goes down.

## Task 3.4 (Bonus) — Scheduled Scaling

Created a scheduled action (`friday-evening-scale-up`) increasing
desired capacity to 6 every Friday at 8:00 PM UTC
(`0 20 * * 5`), to proactively prepare for weekend traffic spikes
before they happen.
