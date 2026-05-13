# AWS  - Observability and Reliability

## About
This project implements monitoring, alerting, and operational runbooks for AWS infrastructure using Amazon CloudWatch and SNS.

The initial observability layer is applied to the Terraform-provisioned environment to validate core monitoring concepts on a controlled infrastructure baseline.

This project focuses on moving beyond deployment by showing how cloud systems can be monitored, diagnosed, and operated under failure conditions.

A planned extension is to apply the same observability model to the previous high-availability architecture, where ALB and Auto Scaling Group metrics provide richer operational signals such as target health, latency, request count, and auto-recovery behaviour.

---

## Objective
Move from:

> “The system is deployed”

to:

> “The system is observable, alertable, and operable.”

This project demonstrates:
- CloudWatch metrics monitoring
- Alarm-based failure detection
- SNS email notifications
- Operational runbooks
- Incident-style documentation
- Failure simulation and validation

---

## Architecture
AWS Infrastructure → CloudWatch Metrics → CloudWatch Alarms → SNS Email Notification → Runbook Response
![Architecture Diagram](docs/architecture-diagram.png)

---

## Core Concepts Demonstrated

| Concept | Explanation |
|---|---|
| Metric | Numerical system measurement over time |
| Alarm | Threshold-based trigger on a metric |
| Dashboard | Visual system health overview |
| SNS | Notification service used for alert delivery |
| Runbook | Step-by-step operational response guide |

---

## Metrics Monitored
| Metric | Source | Purpose |
|---|---|---|
| CPUUtilization | EC2 | Detect workload pressure |
| StatusCheckFailed | EC2 | Detect infrastructure-level failures |
| NetworkIn / NetworkOut | EC2 | Detect unusual traffic patterns |

---

## Alarms Configured

| Alarm | Metric | Threshold | Period | Action |
|---|---|---|---|---|
| week6-high-cpu-alarm | CPUUtilization | > 70% | 5 minutes | SNS email |
| week6-status-check-failed | StatusCheckFailed | > 0 | 1 minute | SNS email |

---
### Why 70% CPU?

A 70% CPU threshold provides early warning before the instance becomes saturated.

A very low threshold, such as 20%, could create excessive false positives under normal workload conditions, contributing to alert fatigue.

This demonstrates the tradeoff between:
- early detection
- signal quality
- alert noise

---

## Dashboard

![CloudWatch Dashboard](screenshots/cloudwatch-dashboard.png)

Dashboard name: `week6-reliability-dashboard`

Widgets include EC2 CPU, status checks and network I/O.

---

## Failure Testing

### CPU Stress Test
Simulated high CPU by running a bash loop across multiple SSH sessions.

Result: CPUUtilization exceeded 70% threshold → alarm entered ALARM state → SNS email received within a few minutes.

![Alarm History](screenshots/alarm-history.png)

---

## Runbooks

Runbooks define the response steps when an alarm fires.

| Runbook | Alarm | Location |
|---|---|---|
| High CPU | week6-high-cpu-alarm | [docs/runbooks/high-cpu-runbook.md](docs/runbooks/high-cpu-runbook.md) |
| Status Check Failure | week6-status-check-failed | [docs/runbooks/status-check-failure-runbook.md](docs/runbooks/status-check-failure-runbook.md) |

---

## Incident Report

A post-incident style write-up for the CPU stress test is documented at:

[docs/incident-reports/cpu-alarm-test.md](docs/incident-reports/cpu-alarm-test.md)

---

## Design Decisions and Tradeoffs
CloudWatch and SNS
CloudWatch and SNS were used because they are native AWS services and integrate directly with EC2 metrics and alarm workflows.
Tradeoff:
This provides strong baseline monitoring, but does not yet include centralised application logs, distributed tracing, or advanced observability tooling.

---
## Static Thresholds
Static thresholds were used for simplicity and clarity.

Tradeoff:
Static alarms are easy to understand, but they may create false positives or miss unusual patterns that anomaly detection could identify.

---

## Production Improvements
Future improvements include:
•	CloudWatch Agent for OS-level and application log shipping 
•	Log metric filters to alert on error patterns in logs 
•	Composite alarms to reduce alert noise 
•	Automated remediation via Lambda on alarm state change 
•	CloudWatch anomaly detection instead of static thresholds 
•	ALB and Auto Scaling Group monitoring for richer service-level signals 

---

## Repository Structure
```
aws-week6-observability-reliability/
├── README.md
├── docs/
│   ├── runbooks/
│   │   ├── high-cpu-runbook.md
│   │   ├── unhealthy-targets-runbook.md
│   │   └── status-check-failure-runbook.md
│   ├── incident-reports/
│   │   └── cpu-alarm-test.md
│   └── architecture-diagram.png
└── screenshots/
├── cloudwatch-dashboard.png
├── cpu-alarm.png
└── alarm-history.png
```
## What I Learned
•	Deployment alone does not make a system reliable 
•	Metrics help identify system behaviour over time 
•	Alarms must be tuned to balance early detection and alert noise 
•	Runbooks turn alerts into actionable operational responses 
•	Observability is a key part of production readiness

---


