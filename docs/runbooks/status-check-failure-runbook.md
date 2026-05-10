# Runbook: EC2 Status Check Failed

## Alarm
week6-status-check-failed — StatusCheckFailed > 0

## What This Means
Either the EC2 instance itself has failed (instance status check)
or the underlying AWS hardware has failed (system status check).
This is an infrastructure-level failure, not an application issue.

## Immediate Steps
1. Go to EC2 → Instances → select instance → Status checks tab
2. Identify whether it is an Instance check or System check failure
3. For System check failure: AWS hardware issue — stop and start the
   instance to migrate to new hardware
4. For Instance check failure: OS/kernel issue — check system log
   via EC2 → Actions → Monitor and troubleshoot → Get system log

## Resolution Options
- System check failure: stop → start the instance (do NOT reboot)
- Instance check failure: reboot first, then replace if unresolved

## Escalate If
- Multiple instances failing simultaneously
- Stop/start does not resolve system check failure
