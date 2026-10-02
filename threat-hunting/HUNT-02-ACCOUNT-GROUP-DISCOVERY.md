# Hunt 02 - Account and Group Discovery Investigation

## Hypothesis
Investigate whether high-volume Windows account and group membership enumeration on LAPTOP-JM2CHLK8 represents unusual or potentially suspicious discovery behavior.

## Data Source
Microsoft Sentinel SecurityEvent table.

## Relevant Events
- 4798 - A user's local group membership was enumerated
- 4799 - Security-enabled local group membership was enumerated
- 4624 - Successful logon
- 4625 - Failed logon
- 4672 - Special privileges assigned to new logon
- 4688 - Process creation

## Baseline
A seven-day baseline identified:

- 6,075 Event ID 4798 records associated with WORKGROUP\LAPTOP-JM2CHLK8$
- 71 Event ID 4798 records associated with LAPTOP-JM2CHLK8\Faraz Shaikh
- 16 Event ID 4799 records associated with WORKGROUP\LAPTOP-JM2CHLK8$
- 9 Event ID 4799 records associated with LAPTOP-JM2CHLK8\Faraz Shaikh

The machine-account context represented the dominant source of discovery telemetry.

## Time-Series Analysis
Discovery events were aggregated into 30-minute intervals.

The activity was bursty rather than evenly distributed.

One significant interval contained 570 Event ID 4798 records associated with the machine-account context between approximately 10:30 and 11:00 UTC on 30 September 2026.

## Correlation
The selected 30-minute window was correlated against:

- Successful logons
- Failed logons
- Privileged logons
- Process creation
- User group discovery
- Local group discovery

The correlation returned:

- 570 Event ID 4798 events - WORKGROUP\LAPTOP-JM2CHLK8$
- 3 Event ID 4624 events - NT AUTHORITY\SYSTEM
- 3 Event ID 4672 events - NT AUTHORITY\SYSTEM
- 2 Event ID 4798 events - LAPTOP-JM2CHLK8\Faraz Shaikh

The query did not return Event ID 4625, 4688, or 4799 records within the selected correlation window.

## Assessment
The investigation confirmed high-volume and bursty group-membership enumeration dominated by the machine-account context.

The reviewed telemetry did not establish malicious activity or identify a malicious process responsible for the enumeration.

The activity demonstrates why high-volume discovery telemetry requires contextual investigation and detection tuning rather than automatic classification as malicious.

## Hunting Lessons
- High event volume does not independently establish malicious behavior.
- Time-series aggregation can reveal behavioral spikes hidden within large datasets.
- Narrow time-window correlation reduces investigation noise.
- Discovery events should be correlated with authentication, privilege, and process telemetry.
- Detection rules should be tuned using observed environmental baselines.
