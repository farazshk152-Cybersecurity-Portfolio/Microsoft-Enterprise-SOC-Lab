# Hunt 03 - Authentication Baseline and Anomaly Hunt

## Hypothesis
Investigate Windows authentication telemetry on LAPTOP-JM2CHLK8 for unusual logon behavior, failed authentication, unexpected logon types, accounts, or source IP addresses.

## Data Source
Microsoft Sentinel SecurityEvent table.

## Relevant Events
- 4624 - Successful logon
- 4625 - Failed logon

## Hunting Method
Authentication events from the previous seven days were grouped by:

- Authentication result
- Account
- Logon type
- Logon type name
- IP address

The objective was to establish the normal authentication baseline and identify unusual patterns requiring further investigation.

## Results
The query returned two authentication patterns:

### NT AUTHORITY\SYSTEM
- Authentication result: Successful
- Event ID: 4624
- Logon Type: 5
- Logon Type Name: Service
- Attempts: 132

### LAPTOP-JM2CHLK8\Faraz Shaikh
- Authentication result: Successful
- Event ID: 4624
- Logon Type: 2
- Logon Type Name: Interactive
- IP Address: 127.0.0.1
- Attempts: 8

No Event ID 4625 failed-logon records were returned by the seven-day hunting query.

## Assessment
The dominant authentication pattern consisted of NT AUTHORITY\SYSTEM service logons.

The local user account generated a smaller number of interactive logons.

Within the collected telemetry reviewed by this hunt, no failed authentication pattern or unexpected logon type was identified that required escalation.

The absence of Event ID 4625 results means no failed-logon records were returned by this query; it should not be interpreted as proof that failed authentication could never have occurred outside the collected telemetry or query scope.

## Hunting Lessons
- Authentication baselines help analysts distinguish expected behavior from anomalies.
- Logon type provides important context for Windows authentication events.
- Service and interactive logons represent different authentication behavior.
- Failed-logon telemetry can be useful for identifying password attacks and authentication anomalies.
- The absence of suspicious results is still a valid threat-hunting outcome when the hypothesis has been tested against available evidence.
