# Microsoft Enterprise SOC Lab - Evidence Index

## Purpose

This document provides an index of the screenshots captured during the Microsoft Enterprise SOC Lab.

The screenshots provide visual evidence of configuration, telemetry validation, detection engineering, incident investigation, identity-security analysis, cloud-security assessment, and threat-hunting activities completed during the project.

Screenshots are organized by project phase under the `screenshots/` directory.

---

## Phase 01 - Azure SOC Foundation

Directory:

`screenshots/phase-01-azure-soc-foundation/`

### 01 - Resource Group Configuration

**File:** `01-resource-group-configuration.png`

Evidence of the Azure resource group configuration used for the Microsoft SOC lab.

### 02 - Log Analytics Workspace Configuration

**File:** `02-log-analytics-workspace-configuration.png`

Evidence of the Log Analytics Workspace configuration used as the central telemetry repository.

### 03 - Log Analytics Workspace Deployed

**File:** `03-log-analytics-workspace-deployed.png`

Evidence that the Log Analytics Workspace deployment completed successfully.

### 04 - Log Analytics Workspace Overview

**File:** `04-log-analytics-workspace-overview.png`

Evidence of the deployed workspace and its Azure configuration.

**Phase 01 Evidence Count: 4**

---

## Phase 02 - Microsoft Sentinel

Directory:

`screenshots/phase-02-sentinel/`

### 01 - Sentinel Workspace Overview

**File:** `01-sentinel-workspace-overview.png`

Evidence of Microsoft Sentinel being associated with the SOC Log Analytics Workspace.

### 02 - Windows Security Events Content Hub

**File:** `02-windows-security-events-content-hub.png`

Evidence of the Windows Security Events solution in Microsoft Sentinel Content Hub.

### 03 - Windows Security Events Solution Deployed

**File:** `03-windows-security-events-solution-deployed.png`

Evidence that the Windows Security Events solution was successfully deployed.

### 04 - Windows Security Events AMA Connector

**File:** `04-windows-security-events-ama-connector.png`

Evidence of the Windows Security Events via AMA data connector.

### 05 - Windows Security Events DCR Review

**File:** `05-windows-security-events-dcr-review.png`

Evidence of reviewing the Data Collection Rule used for Windows Security telemetry.

### 06 - DCR Creation Succeeded

**File:** `06-dcr-creation-succeeded.png`

Evidence that the Data Collection Rule was created successfully.

**Phase 02 Evidence Count: 6**

---

## Phase 03 - Endpoint Telemetry

Directory:

`screenshots/phase-03-telemetry/`

### 01 - Azure Arc Machine Connected

**File:** `01-azure-arc-machine-connected.png`

Evidence that the Windows 11 SOC endpoint `LAPTOP-JM2CHLK8` was successfully connected to Azure Arc.

### 02 - Security Events DCR Association Succeeded

**File:** `02-security-events-dcr-association-succeeded.png`

Evidence that the Windows Security Events Data Collection Rule was associated with the Arc-connected endpoint.

### 03 - Sentinel SecurityEvent KQL Validation

**File:** `03-sentinel-securityevent-kql-validation.png`

Evidence that Windows Security telemetry reached Microsoft Sentinel and could be queried through the `SecurityEvent` table.

This validated the end-to-end telemetry pipeline:

`Windows Endpoint -> Azure Arc -> AMA -> DCR -> Log Analytics -> Microsoft Sentinel`

**Phase 03 Evidence Count: 3**

---

## Phase 04 - KQL Investigation

Directory:

`screenshots/phase-04-kql/`

### 01 - SecurityEvent Behavior Hunting Query

**File:** `01-securityevent-behavior-hunting-query.png`

Evidence of KQL being used to investigate Windows SecurityEvent behavior.

### 02 - Multi-Event SOC Investigation

**File:** `02-multi-event-soc-investigation.png`

Evidence of a multi-event SOC investigation correlating security-relevant Windows event types.

**Phase 04 Evidence Count: 2**

---

## Phase 05 - Detection Engineering

Directory:

`screenshots/phase-05-detection-engineering/`

### 01 - Account and Group Discovery Rule Validation

**File:** `01-account-group-discovery-rule-validation.png`

Evidence of validation for the Microsoft Sentinel analytics rule monitoring Windows account/group discovery activity.

### 02 - Account and Group Discovery Incident Created

**File:** `02-account-group-discovery-incident-created.png`

Evidence that the analytics rule generated a Microsoft Sentinel incident.

The resulting detection contained:

- 42 matching events
- 1 alert
- 1 incident

### 03 - Incident Triaged and Closed

**File:** `03-incident-triaged-and-closed.png`

Evidence of the incident triage and analyst disposition workflow.

The activity was investigated and closed after available telemetry did not establish malicious activity.

### 04 - Tuned Privileged Logon Rule Validation

**File:** `04-tuned-privileged-logon-rule-validation.png`

Evidence of the tuned Non-SYSTEM Privileged Logon analytics rule.

The rule demonstrates noise reduction by excluding routine `NT AUTHORITY\SYSTEM` privileged activity.

**Phase 05 Evidence Count: 4**

---

## Phase 06 - Incident Response

Directory:

`screenshots/phase-06-incident-response/`

No dedicated screenshots were captured for this phase.

Incident-response evidence is documented through the repository artifacts:

- `incident-response/INCIDENT-001-WINDOWS-GROUP-DISCOVERY.md`
- `incident-response/INCIDENT-001-EVIDENCE-TIMELINE.md`
- `incident-response/WINDOWS-DISCOVERY-IR-PLAYBOOK.md`
- `kql/07-incident-timeline-reconstruction.kql`

The incident itself is visually evidenced in the Phase 05 screenshots.

**Phase 06 Evidence Count: 0**

---

## Phase 07 - Microsoft Defender for Cloud

Directory:

`screenshots/phase-07-defender/`

### 01 - Defender for Cloud Security Alerts

**File:** `01-defender-for-cloud-security-alerts.png`

Evidence of Microsoft Defender for Cloud security-alert assessment.

The lab reviewed cloud-security posture using available Foundational CSPM functionality while keeping unnecessary paid Defender plans disabled.

Microsoft Defender for Endpoint and Microsoft Defender XDR were not deployed as part of this lab.

**Phase 07 Evidence Count: 1**

---

## Phase 08 - Microsoft Entra ID Security

Directory:

`screenshots/phase-08-entra/`

### 01 - Interrupted MFA Sign-In Investigation

**File:** `01-entra-interrupted-mfa-signin-investigation.png`

Evidence of investigating an interrupted Azure Portal authentication event in Microsoft Entra ID.

The investigated event required additional multi-factor authentication and returned error code `50074`.

The event was investigated in context rather than automatically classified as malicious.

### 02 - Entra Identity Secure Score

**File:** `02-entra-identity-secure-score.png`

Evidence of Microsoft Entra Identity Secure Score assessment.

The lab tenant showed an Identity Secure Score of **68.42%** during the assessment.

**Phase 08 Evidence Count: 2**

---

## Phase 09 - Threat Hunting

Directory:

`screenshots/phase-09-threat-hunting/`

### 01 - Privileged Logon Session Correlation

**File:** `01-privileged-logon-session-correlation.png`

Evidence of correlating Event ID `4624` with Event ID `4672` using the Windows Logon ID.

The matching identifier demonstrated that the successful interactive authentication and privileged-logon event belonged to the same Windows session.

### 02 - Account and Group Discovery Correlation

**File:** `02-account-group-discovery-correlation.png`

Evidence of threat hunting around high-volume account/group discovery activity using Events `4798` and `4799`.

The hunt used behavioral baselining and time-window correlation to investigate discovery activity.

### 03 - Authentication Baseline Hunt

**File:** `03-authentication-baseline-hunt.png`

Evidence of establishing a Windows authentication baseline using Events `4624` and `4625`.

The hunt compared normal SYSTEM service authentication with local interactive authentication behavior.

**Phase 09 Evidence Count: 3**

---

## Phase 10 - Final Portfolio Integration

Directory:

`screenshots/phase-10-final/`

No screenshot has been added solely for documentation completion.

Phase 10 focuses on:

- Final README
- Architecture documentation
- Evidence indexing
- Repository organization
- Roadmap completion
- Changelog updates
- GitHub portfolio validation

**Phase 10 Evidence Count: 0**

---

## Evidence Summary

| Phase | Area | Screenshots |
|---|---|---:|
| 01 | Azure SOC Foundation | 4 |
| 02 | Microsoft Sentinel | 6 |
| 03 | Endpoint Telemetry | 3 |
| 04 | KQL Investigation | 2 |
| 05 | Detection Engineering | 4 |
| 06 | Incident Response | 0 |
| 07 | Microsoft Defender for Cloud | 1 |
| 08 | Microsoft Entra ID Security | 2 |
| 09 | Threat Hunting | 3 |
| 10 | Final Portfolio Integration | 0 |
| **Total** | | **25** |

---

## What the Evidence Demonstrates

The screenshot collection provides evidence of hands-on work across the SOC lifecycle:

`Azure Foundation -> Endpoint Onboarding -> Telemetry Collection -> KQL Investigation -> Detection Engineering -> Alert -> Incident -> Triage -> Incident Response -> Identity Investigation -> Cloud Security Assessment -> Threat Hunting`

The evidence is supported by KQL queries, detection-rule documentation, incident-response documentation, threat-hunting reports, and architecture documentation stored elsewhere in the repository.

---

## Evidence Integrity

Screenshots are retained as supporting evidence of lab implementation.

They should be interpreted together with the technical documentation and queries in the repository rather than as standalone proof of every configuration detail.

Sensitive information such as credentials, authentication tokens, billing details, subscription identifiers, and tenant identifiers should not be exposed in public repository evidence.