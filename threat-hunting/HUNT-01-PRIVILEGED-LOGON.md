# Hunt 01 - Privileged Logon Session Correlation

## Hypothesis
Investigate whether non-SYSTEM privileged logon events on LAPTOP-JM2CHLK8 represent unusual or potentially suspicious activity.

## Data Source
Microsoft Sentinel SecurityEvent table.

## Relevant Events
- 4624 - Successful logon
- 4672 - Special privileges assigned to new logon

## Baseline
A seven-day baseline identified:
- 132 privileged logon events associated with NT AUTHORITY\SYSTEM
- 4 privileged logon events associated with the local user account

SYSTEM represented the dominant baseline and was excluded from the focused investigation.

## Investigation
One local-user privileged logon was selected and correlated with surrounding authentication telemetry.

The investigation identified:
- Event ID 4624
- Logon Type 2 - Interactive
- TargetLogonId: 0x1080b537
- Event ID 4672
- SubjectLogonId: 0x1080b537

The matching logon identifier linked the successful authentication and privilege-assignment events to the same Windows logon session.

## Assessment
The reviewed telemetry established an interactive local-user logon followed by assignment of special privileges within the same logon session.

The reviewed evidence did not establish malicious activity.

## Hunting Lessons
- Establish a baseline before investigating outliers.
- Remove dominant expected noise carefully.
- Correlate events using session identifiers rather than timestamps alone.
- Event ID 4672 is security-relevant but does not independently prove privilege escalation by an attacker.
