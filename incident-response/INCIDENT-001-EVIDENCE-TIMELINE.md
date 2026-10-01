# Incident 001 - Evidence and Investigation Timeline

## Incident

SOC Lab - Windows Account and Group Discovery

## Affected Asset

Host: LAPTOP-JM2CHLK8

## Detection Source

Microsoft Sentinel scheduled analytics rule using the SecurityEvent table.

## Triggering Telemetry

Windows Security Event IDs monitored:

- 4798 - User local group membership enumerated
- 4799 - Security-enabled local group membership enumerated

The generated incident contained:

- 42 matching security events
- 1 Microsoft Sentinel alert
- 1 Microsoft Sentinel incident

## Investigation Timeline

### 1. Detection

The custom Microsoft Sentinel analytics rule detected matching Windows account/group discovery telemetry.

### 2. Incident Creation

Microsoft Sentinel grouped the matching events into one alert and automatically created an incident.

Initial incident state:

- Severity: Medium
- Status: New
- Host entity: LAPTOP-JM2CHLK8

### 3. Analyst Assignment

The incident was assigned to an analyst and changed from New to Active.

### 4. Evidence Review

The underlying Log Analytics records associated with the alert were reviewed.

Visible matching telemetry consisted of Windows Security Event ID 4798 events originating from LAPTOP-JM2CHLK8.

### 5. Correlation

Surrounding Windows security telemetry was reviewed using KQL.

Relevant security event categories included:

- Authentication
- Privileged logons
- Process creation
- Account/group discovery

The objective was to determine whether additional telemetry supported malicious activity.

### 6. Analyst Assessment

Event ID 4798 confirmed that local group membership enumeration occurred.

However, the event alone did not establish malicious activity.

The investigation did not identify sufficient evidence establishing malicious behavior associated with the detected enumeration.

### 7. Disposition

The activity was treated as expected/benign within the controlled SOC lab.

The incident was documented and closed.

No containment, eradication, or recovery action was required because malicious activity was not established.

## Evidence Preserved

The project preserves:

- Analytics rule configuration
- Detection KQL
- Sentinel incident screenshots
- Underlying event investigation
- KQL correlation queries
- Incident investigation documentation
- Incident response playbook
- Detection tuning documentation

## Lessons Learned

1. Raw security events are not equivalent to confirmed threats.
2. Alerts require contextual validation.
3. Multiple events can be grouped into one actionable alert.
4. Incidents provide a case-management layer above alerts.
5. KQL correlation helps reconstruct activity around a detection.
6. Response actions should be based on validated evidence.
7. Detection rules may require tuning to control alert noise.
8. Investigation findings should be documented before incident closure.
