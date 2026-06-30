INCIDENT REPORT — Unauthorised Access Attempt
═══════════════════════════════════════════════

Incident ID: INC-003
Date: 
Severity: P1 — Critical Security Event
Status: Resolved — Simulated Exercise

TIMELINE
--------
[HH:MM] — Unauthorised API calls initiated
[HH:MM] — CloudTrail captured AccessDenied events
[HH:MM] — CloudWatch metric filter detected pattern
[HH:MM] — CloudOps-UnauthorisedAccess-Detected alarmed
[HH:MM] — SNS notification delivered to security email
[HH:MM] — Investigation confirmed simulated exercise
[HH:MM] — Alarm returned to OK state

ROOT CAUSE
----------
Simulated unauthorised API calls triggering AccessDenied 
errors. In production this would indicate:
- Compromised IAM credentials being used
- Insider threat attempting privilege escalation
- Automated attack tool scanning AWS account
- Misconfigured application using wrong credentials

IMPACT
------
- No actual data breach occurred (simulated)
- CloudTrail audit trail fully intact
- Detection time: under 5 minutes from first call

DETECTION METHOD
----------------
CloudTrail capturing all API calls across all regions
CloudWatch Logs metric filter: $.errorCode=AccessDenied
Alarm threshold: >= 1 occurrence in 5 minutes
Alert delivered via SNS to security email

RESOLUTION STEPS
----------------
1. Received SNS alert for unauthorised access
2. Opened CloudTrail Event History immediately
3. Identified source of AccessDenied calls
4. Confirmed calls originated from CloudShell (simulation)
5. In real incident: would rotate IAM credentials
6. In real incident: would block source IP via NACL
7. Confirmed alarm returned to OK state

PREVENTION
----------
- MFA enforcement on all IAM users
- AWS GuardDuty for advanced threat detection
- IAM Access Analyzer for permission reviews
- SCPs via AWS Organizations for guardrails

Documented by: Saurav Bir
Role: Cloud Operations Engineer
