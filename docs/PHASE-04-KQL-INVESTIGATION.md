# Phase 04 - KQL Fundamentals and SOC Investigation

## Objective

The objective of this phase was to learn Kusto Query Language (KQL) and use it to investigate real Windows Security telemetry collected from the Azure Arc-enabled endpoint LAPTOP-JM2CHLK8.

Rather than only copying queries, this phase focused on understanding how a SOC analyst converts raw Windows events into useful security investigations.

## Data Source

Microsoft Sentinel table:

SecurityEvent

Telemetry path:

Windows 11 Endpoint
? Azure Arc
? Azure Monitor Agent
? Data Collection Rule
? Log Analytics Workspace
? Microsoft Sentinel
? KQL Investigation

## KQL Concepts Practiced

The following KQL operators and functions were used:

- where - filters events
- project - selects useful columns
- take - limits returned records
- order by - sorts results
- summarize - aggregates data
- count() - counts matching events
- distinct - identifies unique values
- bin() - groups events into time intervals
- in() - filters multiple values
- =~ - case-insensitive comparison
- extend - creates calculated columns
- case() - categorizes events

## Windows Security Events Investigated

| Event ID | Meaning |
|----------|---------|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4672 | Special privileges assigned to a new logon |
| 4688 | New process created |
| 4798 | User local group membership enumerated |
| 4799 | Security-enabled local group membership enumerated |

## Investigation Workflow

The investigation followed the SOC workflow:

Security Event
? Identify Event ID
? Identify Account
? Determine Authentication Context
? Review Privilege Activity
? Review Process Activity
? Review Discovery Activity
? Build Timeline
? Determine Whether Activity Requires Further Investigation

An important lesson from this phase was that a security event is not automatically malicious.

For example:

- Event 4624 can represent a normal successful logon.
- Event 4672 can legitimately occur for highly privileged Windows service accounts.
- Event 4798 can occur during legitimate group enumeration.

Context and correlation are required before classifying activity.

## KQL Hunting Queries

Six reusable queries were created:

1. Endpoint Security Event Behavior Analysis
2. Windows Authentication Investigation
3. Privileged Logon Investigation
4. Account and Group Discovery Investigation
5. Windows Process Creation Investigation
6. Multi-Event SOC Investigation Timeline

These queries are stored in the /kql directory.

## Key SOC Skills Practiced

- Windows security event analysis
- Authentication investigation
- Privileged account investigation
- Process execution investigation
- Account and group discovery analysis
- Event aggregation
- Timeline analysis
- KQL-based threat hunting
- Security telemetry interpretation
- Distinguishing telemetry from evidence of malicious activity

## Outcome

Phase 04 established the KQL investigation foundation required for the next stage of the project.

The next phase converts selected KQL logic into Microsoft Sentinel analytics rules so that suspicious activity can generate alerts and incidents automatically.
