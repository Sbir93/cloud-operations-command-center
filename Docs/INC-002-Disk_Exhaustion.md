# INC-002 — Disk Exhaustion
## Incident Report & Root Cause Analysis

**Incident ID:** INC-002
**Date:** June 2026
**Severity:** P1 — Critical
**Status:** ✅ Resolved
**Detected By:** AWS CloudWatch Alarm
**Resolved By:** Saurav Bir — Cloud Operations Engineer

---

## Summary

A disk exhaustion incident was simulated on the
CloudOps-CommandCenter EC2 server by creating a
large file on the root filesystem. Disk usage
spiked from 45% to 75%+, triggering the
CloudOps-DiskSpace-Critical alarm within 3 minutes.
The incident was detected, investigated, and resolved
entirely through AWS monitoring tooling with zero
manual dashboard checking required.

---

## Timeline

| Time | Event |
|---|---|
| T+00:00 | Large 2GB file created on root filesystem |
| T+01:00 | CloudWatch Agent detected disk_used_percent spike |
| T+02:00 | Disk usage crossed 80% critical threshold |
| T+02:30 | CloudOps-DiskSpace-Critical entered IN ALARM state |
| T+03:00 | SNS topic triggered — alert email delivered |
| T+04:00 | Engineer received email — began investigation |
| T+05:00 | Large file located on root filesystem |
| T+06:00 | File removed — disk returned to 45% |
| T+07:00 | CloudWatch alarm returned to OK state |
| T+08:00 | Incident documented — RCA written |

---

## Root Cause

Large dummy file created on root filesystem (/)
consuming available disk space pushing usage
from 45% baseline to 75%+ critical level.

In production this would indicate:
- Application logs growing uncontrolled
- Database transaction logs filling root volume
- Large file uploads not moved to S3
- Backup files accumulating without cleanup
- Docker images filling disk over time
- Core dump files from application crashes

---

## Impact Analysis

| Area | Impact |
|---|---|
| Disk I/O | Performance degraded during incident |
| Application | Write operations would fail at 100% |
| Database | Would crash if unable to write logs |
| Server | Complete failure risk at 100% disk |
| Users | Service unavailable if server crashes |
| Data | Potential loss if DB crashes mid-write |

---

## Detection Method

| Item | Detail |
|---|---|
| Monitoring Tool | AWS CloudWatch |
| Agent | CloudWatch Agent (CWAgent) |
| Metric | disk_used_percent |
| Namespace | CWAgent |
| Alarm Name | CloudOps-DiskSpace-Critical |
| Threshold | > 80% on root volume (/) |
| Evaluation Period | 5 minutes |
| Datapoints | 1 out of 1 |
| Alert Channel | SNS → CloudOps-CommandCenter-Alerts |
| Delivery Method | Email notification |
| Detection Time | < 3 minutes from threshold breach |

---

## Resolution Steps

### Step 1 — Alert Received
- Received SNS email alert
- Subject: CloudOps-DiskSpace-Critical IN ALARM
- Opened CloudWatch Dashboard immediately
- Confirmed disk_used_percent spike on dashboard

### Step 2 — Investigation
- Connected to EC2 via AWS Instance Connect
- Checked disk usage breakdown
- Located large file on root filesystem
- Confirmed file was safe to remove

### Step 3 — Resolution
- Removed large files from root filesystem
- Verified disk returned to normal ~45%
- Confirmed CloudWatch alarm returned to OK
- Documented incident and wrote RCA

---

## Prevention Measures

| Prevention | Implementation |
|---|---|
| Log Rotation | Configure logrotate for all app logs |
| Early Warning | CloudWatch alarm at 70% threshold |
| Automated Cleanup | Lambda to clean /tmp weekly |
| S3 Offloading | Move large files to S3 automatically |
| Volume Expansion | Document EBS expansion runbook |
| Weekly Review | Disk usage trend review scheduled |

---

## Lessons Learned

1. Disk exhaustion is silent until critical —
   proactive monitoring is essential
2. Early warning at 70% gives time to act
   before hitting critical 80%
3. CloudWatch Agent provides disk visibility
   that AWS cannot see natively
4. Automated alerting eliminated need for
   manual dashboard monitoring
5. Having runbook ready reduced resolution
   time significantly

---

## AWS Services Involved

| Service | Role |
|---|---|
| EC2 t3.micro | Affected server |
| CloudWatch Agent | Detected disk_used_percent |
| CloudWatch Alarms | Triggered at 80% threshold |
| SNS | Delivered alert to operations email |
| CloudWatch Dashboard | Visualised disk spike |
| IAM Role | Authorised agent to publish metrics |

---

## Incident Metrics

| Metric | Value |
|---|---|
| Time to Detect | < 3 minutes |
| Time to Notify | < 3 minutes |
| Time to Resolve | < 8 minutes |
| Total Downtime | 0 minutes |
| Data Loss | None |
| Users Affected | 0 (simulation) |

---

## Evidence Screenshots

![Disk Normal Before](../screenshots/18-disk-normal-before.png)
*Figure 1: Disk at normal 45% baseline before incident*

![Disk Spike Graph](../screenshots/19-disk-spike-graph.png)
*Figure 2: CloudWatch graph showing disk usage spike*

![Disk Alarm Triggered](../screenshots/20-disk-alarm-triggered.png)
*Figure 3: DiskSpace-Critical alarm IN ALARM state*

![Disk Alarm Email](../screenshots/21-disk-alarm-email.png)
*Figure 4: SNS alert email received in inbox*

![Disk Alarm Resolved](../screenshots/22-disk-alarm-resolved.png)
*Figure 5: Alarm returned to OK after resolution*

---

## Related Incidents

- [INC-001 CPU Spike](INC-001-CPU-Spike-RCA.md)
- [INC-003 Unauthorised Access](INC-003-Unauthorised-Access-RCA.md)
- [INC-004 Budget Breach](INC-004-Budget-Breach-RCA.md)

---

**Documented by:** Saurav Bir
**Role:** Cloud Operations Engineer
**Account:** AWS Free Tier — ap-south-1 Mumbai
**Project:** Cloud Operations Command Center
**GitHub:** github.com/Sbir93