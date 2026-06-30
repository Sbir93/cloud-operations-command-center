INCIDENT REPORT — Budget Breach
═══════════════════════════════════════════════

Incident ID: INC-004
Date:
Severity: P2 — High Financial Impact
Status: Resolved

TIMELINE
--------
[HH:MM] — Budget threshold breached
[HH:MM] — AWS Budgets detected overspend
[HH:MM] — SNS notification triggered
[HH:MM] — Budget alert email delivered
[HH:MM] — Investigation identified cause
[HH:MM] — Budget restored to $20 limit
[HH:MM] — Remediation actions documented

ROOT CAUSE
----------
Simulated budget breach by temporarily lowering 
budget threshold below current spend ($3.36).
In production this would indicate:
- Forgotten running EC2 instances
- Undeleted RDS snapshots accumulating
- NAT Gateway data transfer charges
- Uncompressed S3 storage growing unchecked

IMPACT
------
- No actual financial loss (credits account)
- Alert system confirmed working end to end
- Detection time: under 10 minutes

DETECTION METHOD
----------------
AWS Budgets monitoring actual vs budgeted spend
Alert thresholds: 50% / 80% / 100%
SNS notification → email delivery confirmed

RESOLUTION STEPS
----------------
1. Received budget breach email alert
2. Opened AWS Cost Explorer immediately
3. Identified which services driving cost
4. In real incident: would stop non-essential resources
5. In real incident: would implement resource tagging
6. Restored budget to correct $20 threshold
7. Added 50% early warning threshold as prevention

PREVENTION
----------
- Cost Allocation Tags on all resources
- Weekly Cost Explorer review scheduled
- Auto-shutdown Lambda for non-prod resources
- Reserved Instances for predictable workloads

Documented by: Saurav Bir
Role: Cloud Operations Engineer
