# Runbook: No Healthy ALB Targets

## Alarm
week6-alb-no-healthy-hosts — HealthyHostCount < 1

## Severity
P1 — Service is completely down

## Immediate Steps
1. Go to EC2 → Target Groups → Targets tab
2. Check which instances are unhealthy and why
3. Check EC2 instance status — are they running?
4. Check security group — does it allow port 80 from the ALB SG?
5. Check the application — is the process running on the instance?
6. Check ASG activity tab — is it trying to replace instances?

## Resolution Options
- If SG misconfiguration: fix inbound rules
- If app crashed: restart the service on the instance
- If instance failure: let ASG replace it (verify ASG is active)

## Escalate If
- All instances failing and ASG is not replacing them
- Health checks failing for an unknown reason
