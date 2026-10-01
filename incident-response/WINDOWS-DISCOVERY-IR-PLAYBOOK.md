# Incident Response Playbook - Suspicious Windows Discovery Activity

## Purpose

This playbook defines the investigation and response workflow for suspicious Windows account or group discovery activity detected by Microsoft Sentinel.

## 1. Detection

Review the Sentinel analytics rule and associated alert.

Validate:

- Detection timestamp
- Windows Event ID
- Host
- Account
- Alert severity
- Mapped entities
- Related events

## 2. Triage

Determine whether the activity requires investigation.

Review:

- Event frequency
- User/account context
- Host context
- Authentication activity
- Privileged logons
- Process execution
- Discovery activity
- Historical activity

Do not classify Event IDs 4798 or 4799 as malicious based only on their presence.

## 3. Investigation

Use KQL to build a timeline around the detection.

Correlate:

- 4624 - Successful Logon
- 4625 - Failed Logon
- 4672 - Special Privileges Assigned
- 4688 - Process Creation
- 4798 - User Local Group Membership Enumerated
- 4799 - Local Group Membership Enumerated

Determine:

- Which account performed the activity?
- Which endpoint was involved?
- What occurred before the discovery activity?
- What occurred afterward?
- Was unusual process execution observed?
- Was privileged access involved?
- Is the activity expected for the user or system?

## 4. Containment

If malicious activity is confirmed or containment is otherwise justified, evaluate actions such as:

- Isolate the affected endpoint
- Disable or restrict compromised accounts
- Revoke active sessions
- Block confirmed malicious infrastructure
- Stop confirmed malicious processes
- Prevent lateral movement

Containment actions should be proportional to the evidence and operational impact.

## 5. Eradication

After containment:

- Remove confirmed malicious files or persistence
- Remove unauthorized accounts
- Remove malicious scheduled tasks or services
- Reset compromised credentials
- Patch exploited vulnerabilities
- Correct security misconfigurations

## 6. Recovery

Before returning systems to normal operation:

- Validate endpoint integrity
- Restore required services
- Re-enable accounts only when appropriate
- Reconnect isolated endpoints after validation
- Confirm security tooling is operational
- Monitor for recurrence

## 7. Post-Incident Review

Document:

- Root cause
- Initial access where known
- Affected assets
- Accounts involved
- Timeline
- Detection source
- Response actions
- Detection gaps
- Lessons learned

## 8. Detection Improvement

Determine whether:

- Analytics rules require tuning
- Additional telemetry is needed
- Entity mapping should be improved
- Thresholds should change
- Known benign activity should be carefully excluded
- New hunting queries or detections should be created

## SOC Principle

An alert is the beginning of an investigation, not proof of compromise.

Response actions should be based on validated evidence and business impact.
