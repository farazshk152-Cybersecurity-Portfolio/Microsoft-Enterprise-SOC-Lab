# Microsoft Enterprise SOC Lab - Roadmap

## Project Status

**Technical SOC implementation: Complete**

**Final portfolio integration: In Progress**

The original roadmap evolved during implementation to reflect licensing availability, cost constraints, available telemetry, and the goal of maintaining an accurate hands-on portfolio.

The final project prioritizes Microsoft Sentinel, Windows security telemetry, KQL, detection engineering, incident response, Microsoft Entra ID security, Microsoft Defender for Cloud, and threat hunting.

---

## Phase 00 - Environment Readiness

- [x] Verify Microsoft Entra tenant
- [x] Configure dedicated Azure subscription
- [x] Configure Azure cost monitoring
- [x] Configure budget alerting
- [x] Establish resource naming convention
- [x] Establish tagging strategy
- [x] Create GitHub repository structure

**Status: Complete**

---

## Phase 01 - Azure SOC Foundation

- [x] Create `rg-microsoft-soc-lab`
- [x] Select UAE North deployment region
- [x] Apply resource tags
- [x] Deploy `law-microsoft-soc-lab`
- [x] Validate Log Analytics Workspace
- [x] Enable Microsoft Sentinel

**Status: Complete**

---

## Phase 02 - Microsoft Sentinel

- [x] Configure Microsoft Sentinel
- [x] Review Sentinel architecture
- [x] Review Content Hub
- [x] Install Windows Security Events solution
- [x] Configure Windows Security Events via AMA
- [x] Configure Data Collection Rule
- [x] Review Sentinel telemetry flow
- [x] Validate connector configuration

**Status: Complete**

---

## Phase 03 - Windows Security Telemetry

- [x] Connect physical Windows 11 endpoint using Azure Arc
- [x] Install Azure Connected Machine Agent
- [x] Validate Azure Arc connectivity
- [x] Deploy Azure Monitor Agent
- [x] Associate endpoint with Data Collection Rule
- [x] Configure focused Windows Security Event collection
- [x] Reduce unnecessary event ingestion
- [x] Validate Windows Security Event ID 4798 locally
- [x] Validate Event ID 4798 in Microsoft Sentinel
- [x] Confirm end-to-end telemetry pipeline

**Status: Complete**

Telemetry pipeline:

`Windows 11 -> Azure Arc -> AMA -> DCR -> Log Analytics -> Microsoft Sentinel`

---

## Phase 04 - KQL Investigation

- [x] Query the `SecurityEvent` table
- [x] Filter security events
- [x] Use `project`
- [x] Use `extend`
- [x] Use `summarize`
- [x] Perform time-based analysis
- [x] Investigate Windows authentication activity
- [x] Investigate privileged logons
- [x] Investigate account/group discovery
- [x] Investigate process-creation telemetry
- [x] Build multi-event investigation query
- [x] Create reusable KQL query library

**Status: Complete**

---

## Phase 05 - Detection Engineering

- [x] Build Sentinel analytics rule for account/group discovery
- [x] Configure rule severity
- [x] Configure host entity mapping
- [x] Configure custom alert details
- [x] Configure event grouping
- [x] Enable incident creation
- [x] Validate analytics-rule results
- [x] Generate Sentinel incident from matching telemetry
- [x] Build Non-SYSTEM Privileged Logon detection
- [x] Exclude expected SYSTEM activity
- [x] Validate tuned detection logic
- [x] Document detection rules

**Status: Complete**

Implemented detections:

1. `SOC Lab - Windows Account and Group Discovery`
2. `SOC Lab - Non-SYSTEM Privileged Logon`

---

## Phase 06 - Incident Response

- [x] Investigate Sentinel incident
- [x] Validate underlying alert
- [x] Review matching events
- [x] Identify affected host
- [x] Review account context
- [x] Correlate authentication and privilege events
- [x] Reconstruct investigation timeline
- [x] Determine available evidence did not establish malicious activity
- [x] Document analyst disposition
- [x] Close investigated incident
- [x] Create Windows Discovery IR playbook
- [x] Create incident evidence timeline
- [x] Create reusable timeline reconstruction query

**Status: Complete**

Incident result:

**42 events -> 1 alert -> 1 Sentinel incident -> analyst investigation -> expected/benign disposition**

---

## Phase 07 - Microsoft Defender

### Implemented

- [x] Access Microsoft Defender for Cloud
- [x] Review Foundational CSPM
- [x] Review security recommendations
- [x] Review resource inventory
- [x] Review security alerts
- [x] Review Defender plan configuration
- [x] Preserve minimal-cost configuration
- [x] Document Defender for Cloud assessment

### Not Implemented

- [ ] Microsoft Defender for Endpoint deployment
- [ ] Microsoft Defender XDR hands-on implementation
- [ ] MDE Device Timeline
- [ ] MDE Live Response
- [ ] Defender XDR Advanced Hunting

Defender XDR portal access was unavailable in the lab tenant. Paid Defender functionality was not enabled solely for portfolio demonstration.

**Implemented Scope Status: Complete**

---

## Phase 08 - Microsoft Entra ID Security

- [x] Review Entra tenant security overview
- [x] Investigate interactive sign-in logs
- [x] Investigate interrupted MFA sign-in
- [x] Analyze authentication requirements
- [x] Review error code 50074
- [x] Review Identity Secure Score
- [x] Review identity-security recommendations
- [x] Review Global Administrator assignment
- [x] Discuss least-privilege implications
- [x] Document identity investigation

### Out of Scope / Not Artificially Implemented

- [ ] Create unnecessary additional lab identities
- [ ] Create unnecessary security groups
- [ ] Modify administrative roles solely for demonstration
- [ ] Deploy licensing-dependent Conditional Access scenarios

**Status: Complete within available lab scope**

---

## Phase 09 - Threat Hunting

- [x] Define threat-hunting methodology
- [x] Build hunting hypotheses
- [x] Establish behavioral baselines
- [x] Perform privileged-logon hunt
- [x] Correlate Event IDs 4624 and 4672
- [x] Correlate Windows Logon IDs
- [x] Perform account/group discovery hunt
- [x] Investigate high-volume 4798 activity
- [x] Perform authentication baseline hunt
- [x] Analyze 4624/4625 telemetry
- [x] Document analyst conclusions
- [x] Identify detection-tuning opportunities
- [x] Create reusable hunting queries
- [x] Create three threat-hunting reports

**Status: Complete**

Completed hunts:

1. Privileged Logon Investigation
2. Account and Group Discovery Investigation
3. Authentication Baseline

---

## Phase 10 - Final Portfolio Integration

- [x] Finalize portfolio README
- [x] Document completed SOC architecture
- [x] Create evidence/screenshot index
- [ ] Update project roadmap
- [ ] Update project changelog
- [ ] Perform final repository audit
- [ ] Verify GitHub rendering
- [ ] Finalize project status
- [ ] Create resume-ready project description
- [ ] Create interview-ready project explanation

**Status: In Progress**

---

## Scope Decisions

Several original roadmap items were intentionally modified or removed during implementation.

### Multi-Stage Attack Simulation

The original roadmap proposed generating a large multi-stage attack scenario.

This was not required for the final lab because the project already contained real collected Windows telemetry sufficient to demonstrate:

- Detection engineering
- Alert generation
- Incident creation
- Incident triage
- Event correlation
- Timeline reconstruction
- Threat hunting

Avoiding unnecessary event generation also supported the project's cost-conscious ingestion strategy.

### Microsoft Defender XDR / Defender for Endpoint

These technologies were part of the original learning plan but were not deployed because the available tenant did not provide the required Defender XDR/MDE environment.

Microsoft Defender for Cloud was used instead for the implemented cloud-security portion.

The repository does not claim MDE/XDR implementation.

### Microsoft Entra Lab Identities

Additional users and groups were not created simply to increase project scope.

Existing Entra telemetry was used for:

- Sign-in investigation
- MFA analysis
- Secure Score assessment
- Privileged-role review

This kept the lab focused on meaningful security analysis rather than artificial configuration volume.

---

## Final Project Outcome

The implemented SOC workflow is:

`Windows Endpoint -> Azure Arc -> Azure Monitor Agent -> Data Collection Rule -> Log Analytics -> Microsoft Sentinel -> KQL -> Detection -> Alert -> Incident -> Investigation -> Incident Response -> Threat Hunting`

Additional security visibility was provided through:

- Microsoft Entra ID
- Microsoft Defender for Cloud

The project demonstrates practical SOC analyst workflows while maintaining clear boundaries between technologies that were actually implemented and technologies that were only considered during the original roadmap.