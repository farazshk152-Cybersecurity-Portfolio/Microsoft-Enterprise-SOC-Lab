    # Microsoft Enterprise SOC Lab — Portfolio Summary

## Project Overview

The Microsoft Enterprise SOC Lab is a hands-on security operations project designed to simulate core SOC Analyst responsibilities using the Microsoft security ecosystem.

The lab demonstrates practical experience with:

- Microsoft Sentinel
- Microsoft Entra ID
- Microsoft Defender for Cloud
- Azure Arc
- Azure Monitor Agent
- Log Analytics
- Data Collection Rules
- Windows Security Events
- KQL
- Detection Engineering
- Alert and Incident Investigation
- Incident Response
- Threat Hunting
- Security Monitoring
- Cost-conscious telemetry management

The project was intentionally designed as a small but realistic SOC environment rather than a high-volume attack simulation.

---

## Architecture

The primary telemetry pipeline is:

Windows 11 Endpoint
→ Azure Arc
→ Azure Monitor Agent
→ Data Collection Rule
→ Log Analytics Workspace
→ Microsoft Sentinel
→ KQL Investigation
→ Analytics Rules
→ Alerts
→ Incidents
→ Investigation / Response / Threat Hunting

Microsoft Entra ID was used for identity-security investigation.

Microsoft Defender for Cloud was used for cloud-security posture and alert assessment.

Microsoft Defender for Endpoint and Microsoft Defender XDR were not deployed as part of this project.

---

## Windows Endpoint Integration

A physical Windows 11 endpoint was connected to Azure using Azure Arc.

The Azure Connected Machine Agent was installed and the endpoint was registered as:

`LAPTOP-JM2CHLK8`

Azure Monitor Agent was then deployed to collect Windows Security telemetry.

A Data Collection Rule controlled which Windows events were forwarded to Log Analytics.

---

## Cost-Conscious Telemetry Collection

Instead of forwarding the entire Windows Security log, the Data Collection Rule was tuned to collect selected security-relevant events.

Examples included:

- 1102 — Security audit log cleared
- 4624 — Successful logon
- 4625 — Failed logon
- 4672 — Special privileges assigned
- 4688 — Process creation
- 4720 — User account created
- 4722 — User account enabled
- 4725 — User account disabled
- 4726 — User account deleted
- 4728 / 4732 / 4756 — Group membership changes
- 4798 — User's local group membership enumerated
- 4799 — Security-enabled local group membership enumerated

This reduced unnecessary ingestion while retaining useful SOC telemetry.

---

## KQL Investigation

A reusable KQL investigation library was created.

The project contains ten KQL files covering:

1. Endpoint SecurityEvent behavior
2. Windows authentication investigation
3. Privileged logon investigation
4. Account/group discovery investigation
5. Process creation investigation
6. Multi-event SOC investigation
7. Incident timeline reconstruction
8. Privileged-logon threat hunting
9. Account/group-discovery threat hunting
10. Authentication-baseline threat hunting

KQL operations used included filtering, projection, field extension, aggregation, time-window analysis, behavioral baselining, and event correlation.

---

## Detection Engineering

Two Microsoft Sentinel analytics rules were implemented.

### Windows Account and Group Discovery

The first rule detected Windows Event IDs 4798 and 4799.

It included:

- KQL detection logic
- Medium severity
- Host entity mapping
- Custom alert details
- Scheduled execution
- Event grouping
- Incident creation

The rule generated a real Sentinel incident from collected endpoint telemetry.

### Non-SYSTEM Privileged Logon

The second rule monitored Event ID 4672 while excluding the highly repetitive:

`NT AUTHORITY\SYSTEM`

This demonstrated detection tuning and noise reduction rather than simply alerting on every privileged-logon event.

---

## Sentinel Incident Investigation

The account/group discovery analytics rule produced:

42 matching events
→ 1 Sentinel alert
→ 1 Sentinel incident

The incident was investigated using:

- Host context
- Account context
- Windows Event IDs
- Authentication activity
- Privileged activity
- Discovery activity
- Time-based correlation

The investigation did not establish evidence of malicious activity.

The incident was therefore documented and closed with an expected/benign disposition rather than being incorrectly labeled as an attack.

---

## Incident Response

A reusable Windows discovery incident-response playbook was created.

The workflow covers:

Detection
→ Triage
→ Investigation
→ Containment Decision
→ Eradication
→ Recovery
→ Lessons Learned
→ Detection Improvement

A dedicated evidence timeline was also created to reconstruct the investigated incident.

The project intentionally avoided performing fake containment actions when the available evidence did not justify them.

---

## Microsoft Defender for Cloud

Microsoft Defender for Cloud was reviewed as the cloud-security layer.

The project examined:

- Foundational CSPM
- Security recommendations
- Resource inventory
- Security alerts
- Defender plan configuration

Paid Defender plans were intentionally left disabled to maintain the low-cost lab design.

Microsoft Defender for Endpoint and Microsoft Defender XDR were not implemented, and the project does not claim hands-on deployment of those products.

---

## Microsoft Entra ID Investigation

Microsoft Entra ID sign-in activity was investigated.

One Azure Portal sign-in showed:

- Status: Interrupted
- Error code: 50074
- Authentication requirement: MFA

The investigation demonstrated that an interrupted authentication event should not automatically be interpreted as compromise.

Identity Secure Score was also reviewed.

The observed score during the lab was:

`68.42%`

Administrative role assignments and least-privilege considerations were also examined.

---

## Threat Hunting

Three structured threat hunts were completed.

### Hunt 01 — Privileged Logon Investigation

Event ID 4672 was baselined.

Observed over the investigated period:

- `NT AUTHORITY\SYSTEM` — 132 privileged logons
- Local user — 4 privileged logons

A local interactive authentication event (4624) was correlated with a privileged-logon event (4672) using the Windows Logon ID.

This demonstrated session-level event correlation.

### Hunt 02 — Account and Group Discovery

Events 4798 and 4799 were analyzed.

A large amount of machine-account discovery telemetry was identified.

A selected time window contained:

- 570 machine-account Event ID 4798 records
- 3 SYSTEM successful logons
- 3 SYSTEM privileged-logon events
- 2 local-user Event ID 4798 records

The activity demonstrated why event volume alone cannot establish malicious behavior.

### Hunt 03 — Authentication Baseline

Successful and failed authentication telemetry was analyzed.

Observed successful authentication included:

- SYSTEM service logons
- Local interactive user logons

No Event ID 4625 records were returned by the selected collected/query telemetry during the investigated period.

This was documented as an observation rather than proof that failed authentication had never occurred.

---

## SOC Skills Demonstrated

The project demonstrates hands-on exposure to:

- SIEM operations
- Windows telemetry
- Microsoft Sentinel
- KQL
- Log Analytics
- Azure Arc
- Azure Monitor Agent
- Data Collection Rules
- Detection engineering
- Alert triage
- Incident investigation
- Event correlation
- Incident response
- Threat hunting
- Authentication analysis
- Privileged-access analysis
- Microsoft Entra ID
- Microsoft Defender for Cloud
- Security baselining
- Detection tuning
- False-positive/noise reduction
- Evidence documentation
- SOC reporting

---

## Key Lessons

The most important lesson from the project is that SOC analysis is not simply about generating alerts.

An analyst must understand:

- Why an event occurred
- Which account generated it
- Which endpoint was involved
- What happened before and after it
- Whether multiple events belong to the same session
- Whether behavior differs from the baseline
- Whether the available evidence actually supports escalation

The project therefore focused on evidence-based investigation rather than automatically treating security events as attacks.

---

## Project Scope and Limitations

This is a controlled portfolio lab rather than a production SOC.

The environment intentionally uses:

- One primary Windows endpoint
- Focused telemetry collection
- Limited Azure resources
- Controlled data ingestion
- No artificial high-volume attack generation
- No Microsoft Defender for Endpoint deployment
- No Microsoft Defender XDR deployment
- Limited Entra ID identities

These limitations were intentional to maintain cost control while still demonstrating core SOC workflows.

---

## Interview Summary

In this project, I built a Microsoft-focused SOC environment around Microsoft Sentinel.

I connected a Windows 11 endpoint to Azure using Azure Arc, deployed Azure Monitor Agent, and used a Data Collection Rule to send selected Windows Security Events into a Log Analytics workspace connected to Sentinel.

I then used KQL to investigate authentication, privileged logons, account and group discovery, process activity, and correlated security events.

I created two Sentinel analytics rules. One detected Windows account/group discovery and generated an actual Sentinel incident. I investigated the incident, reconstructed its timeline, correlated the surrounding telemetry, and documented why the evidence did not establish malicious activity.

I also created an incident-response playbook and performed three structured threat hunts covering privileged activity, account/group discovery, and authentication behavior.

Finally, I investigated Microsoft Entra ID sign-in activity and Identity Secure Score and reviewed Microsoft Defender for Cloud for cloud-security posture and alerts.

The main goal was not simply to generate alerts, but to practice the complete SOC workflow from telemetry collection through detection, investigation, response, threat hunting, tuning, and documentation.