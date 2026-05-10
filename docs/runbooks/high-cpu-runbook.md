# Runbook: High CPU Alarm

## Alarm
week6-high-cpu-alarm — CPUUtilization > 70% for 5 minutes

## Likely Causes
- Unexpected traffic spike
- Runaway process or infinite loop
- Insufficient instance size for current load

## Immediate Steps
1. Go to CloudWatch → check CPU graph — is it still rising or stabilizing?
2. SSH into the instance: `top` or `htop` to find the process consuming CPU
3. Check recent deployments — did CPU spike after a push?
4. Check ALB RequestCount — is traffic higher than normal?

## Resolution Options
- If runaway process: kill the process (`kill -9 <PID>`)
- If traffic spike: manually increase ASG desired capacity
- If persistent: consider upgrading instance type

## Escalate If
- CPU stays above 90% for more than 15 minutes
- Application becomes unresponsive
