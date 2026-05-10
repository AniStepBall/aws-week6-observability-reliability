# Incident Simulation — High CPU Alarm

**Date:** [today's date]
**Severity:** Simulated / Non-production

## Scenario
Simulated high CPU usage on EC2 instance using a bash loop across
multiple terminal sessions to drive CPUUtilization above the 70%
threshold.

## Expected Behavior
CloudWatch alarm should transition to ALARM state within 5 minutes
and send an SNS email notification.

## What Happened
CPU utilization increased above 70% threshold. After one 5-minute
evaluation period, the alarm transitioned from OK to ALARM state.
An SNS email notification was received within 2 minutes of the
state change.

## Detection
CloudWatch alarm (week6-high-cpu-alarm) and SNS email notification.

## Response
Identified the bash loop processes causing the CPU spike and
terminated them using Ctrl+C. CPU returned to baseline within
2 minutes. Alarm transitioned back to OK state.

## Lessons Learned
- Monitoring works as expected end-to-end
- A 5-minute evaluation window prevents false positives from
  short-lived spikes
- Runbooks should be accessible before an incident, not after
- In production, this alarm would also trigger auto-scaling
