# Windows Account and Group Discovery Detection

## Detection Name

SOC Lab - Windows Account and Group Discovery

## Objective

Detect Windows local user or security-enabled local group membership enumeration on the monitored SOC endpoint.

## Data Source

Microsoft Sentinel SecurityEvent table.

## Windows Event IDs

- 4798 - A user's local group membership was enumerated
- 4799 - A security-enabled local group membership was enumerated

## Detection Query

SecurityEvent
| where EventID in (4798, 4799)
| where Computer =~ "LAPTOP-JM2CHLK8"
| project TimeGenerated, Computer, EventID, Activity, Account, SubjectAccount

## Rule Configuration

- Rule type: Scheduled analytics rule
- Severity: Medium
- Status: Enabled
- Query frequency: 1 hour
- Lookback period: 1 hour
- Alert threshold: More than 0 results
- Event grouping: Group all events into a single alert
- Suppression: Disabled
- Incident creation: Enabled
- Alert grouping: Disabled
- Automated response: Not configured

## Entity Mapping

Host:
- HostName -> Computer

## Custom Details

- EventID -> EventID
- Activity -> Activity
- Account -> Account

## Detection Engineering Notes

Events 4798 and 4799 are not inherently malicious. Legitimate Windows components, administrative activity, applications, and security tooling can enumerate group membership.

The detection therefore provides investigation context rather than proving compromise by itself. An analyst should correlate the activity with the account, host, process activity, authentication events, privilege events, and surrounding timeline before classification.

## Investigation Workflow

4798 / 4799 detected
-> Identify host
-> Identify associated account
-> Review authentication activity
-> Review privileged activity
-> Review process execution
-> Review surrounding discovery activity
-> Determine whether behavior is expected
-> Classify and close or escalate the incident
