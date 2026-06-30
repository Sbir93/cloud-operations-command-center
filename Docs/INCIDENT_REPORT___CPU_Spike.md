INCIDENT REPORT — CPU Spike
═══════════════════════════════════════════════

Incident ID: INC-001
Date: June 2026
Severity: P2 — High
Status: Resolved

TIMELINE
--------
[HH:MM] — CPU spike initiated via stress test
[HH:MM] — CloudWatch detected CPU > 80%
[HH:MM] — CloudOps-HighCPU-Alert entered IN ALARM state
[HH:MM] — SNS notification delivered to email
[HH:MM] — Stress test terminated
[HH:MM] — CPU returned to normal baseline
[HH:MM] — Alarm returned to OK state

ROOT CAUSE
----------
Simulated runaway process consuming 100% CPU across 
2 cores. In production this would indicate:
- Application infinite loop
- Cryptocurrency miner malware
- Unoptimised database query
- DDoS amplification attack

IMPACT
------
- Server response time degraded during spike
- No data loss occurred
- No service outage (t3.micro single instance)

DETECTION METHOD
----------------
CloudWatch CWAgent metric: cpu_usage_user
Alarm threshold: > 80% for 2 consecutive minutes
Alert delivered via SNS to operations email

RESOLUTION STEPS
----------------
1. Identified spike via CloudWatch Dashboard
2. Located stress process using: top command
3. Terminated process: sudo pkill stress
4. Confirmed CPU return to baseline (<5%)
5. Confirmed alarm state returned to OK

PREVENTION
----------
- Auto Scaling Group would prevent single instance overload
- Lambda function could auto-terminate runaway processes
- Process monitoring via CloudWatch agent enhancements

Documented by: Saurav Bir
Role: Cloud Operations Engineer
