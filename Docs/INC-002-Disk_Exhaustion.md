# INC-002 — Disk Exhaustion
## Incident Report & Root Cause Analysis

**Incident ID:** INC-002  
**Date:** June 2026  
**Severity:** P1 — Critical  
**Status:** ✅ Resolved  

---

## Timeline
| Time | Event |
|---|---|
| T+00:00 | Large file created on root filesystem |
| T+02:00 | Disk usage crossed 80% threshold |
| T+02:30 | CloudOps-DiskSpace-Critical entered IN ALARM |
| T+03:00 | SNS notification delivered to operations email |
| T+05:00 | Large file identified and removed |
| T+06:00 | Disk returned to normal baseline (~45%) |
| T+07:00 | Alarm returned to OK state |

---

## Root Cause
4GB dummy file created on root filesystem consuming 
available disk space pushing usage from 45% to 75%+.

In production this indicates:
- Application logs growing uncontrolled
- Database transaction logs not rotating
- Large file uploads not being processed
- Backup files accumulating without cleanup

---

## Impact
- Disk I/O performance degraded during incident
- Risk of complete server failure at 100% disk
- Application write operations would fail at 100%
- Database would crash if unable to write logs

---

## Detection Method