# Incident 001 - Windows Account and Group Discovery

## Incident Summary

A Microsoft Sentinel scheduled analytics rule detected Windows local group membership enumeration activity on the monitored endpoint LAPTOP-JM2CHLK8.

The rule generated one Medium-severity Sentinel incident containing 42 matching security events.

## Detection

Analytics Rule:
SOC Lab - Windows Account and Group Discovery

Windows Event IDs monitored:
- 4798 - A user's local group membership was enumerated
- 4799 - A security-enabled local group membership was enumerated

## Initial Triage

The incident was:

- Severity: Medium
- Initial status: New
- Assigned to an analyst
- Status changed to Active for investigation
- Host entity: LAPTOP-JM2CHLK8
- Alerts: 1
- Events: 42

## Evidence Review

The underlying Log Analytics results were reviewed.

The visible matching telemetry consisted of Windows Security Event ID 4798 events from LAPTOP-JM2CHLK8.

Observed account contexts included the local user account and machine-account context.

The events represented local group membership enumeration activity.

## Investigation

The alert was correlated with surrounding Windows security telemetry using KQL.

Relevant event categories reviewed included:

- 4624 - Successful Logon
- 4625 - Failed Logon
- 4672 - Privileged Logon
- 4688 - Process Creation
- 4798 - User Group Discovery
- 4799 - Local Group Discovery

The investigation did not establish evidence of malicious activity associated with the detected enumeration.

## Analysis

Event IDs 4798 and 4799 are useful discovery indicators but are not inherently malicious.

Legitimate operating-system activity, administrative operations, applications, and security tools may enumerate local group membership.

Therefore, these events require contextual investigation rather than automatic classification as malicious activity.

## Disposition

The activity was treated as expected/benign within the controlled SOC lab environment.

The incident was closed after investigation.

## Detection Engineering Lesson

The analytics rule successfully detected the intended telemetry.

42 matching events were grouped into:

42 Security Events
-> 1 Sentinel Alert
-> 1 Sentinel Incident

This demonstrated the difference between raw telemetry, alerts, and incidents.

The investigation also demonstrated why detection logic must be tuned to reduce noise and why analysts must correlate alerts with surrounding telemetry before determining disposition.

## SOC Workflow Demonstrated

Detection
-> Alert
-> Incident
-> Assignment
-> Triage
-> Evidence Review
-> KQL Correlation
-> Analysis
-> Classification
-> Closure
