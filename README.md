# AWS Week 6 - Observability and Reliability

## About
This project implements monitoring, alerting, and operational runbooks for AWS infrastructure using CloudWatch and SNS.

This observability layer was applied to the Week 5 Terraform-only environment. However, it is better to use Week 2 high-availability since the combination of ALB + ASG systems provides a richer signals, including target health, latency, request count, and auto-recovery behaviour. 

Week 6 focuses on operating and monitoring a live multi-component system.

---

## Objective
Move beyond deployment and demonstrate how cloud systems can be monitored, diagnosed, and operated under failure conditions.

---

## Architecture
![Architecture Diagram](docs/architecture-diagram.png)

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

### Why 70% CPU?
70% was chosen to provide headroom before the instance becomes saturated, giving time to investigate before performance degrades.
A threshold of 20% would generate constant false positives under normal load.

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

A post-incident style writeup for the CPU stress test is documented at:

[docs/incident-reports/cpu-alarm-test.md](docs/incident-reports/cpu-alarm-test.md)

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

---

## Future Improvements

- CloudWatch Agent for OS-level and application log shipping
- Log metric filters to alert on error patterns in logs
- Composite alarms to reduce alert noise
- Automated remediation via Lambda on alarm state change
- Anomaly detection alarms instead of static thresholds
