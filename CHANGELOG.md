# Changelog

## 2026-09-30

### Project Initialization

- Created Microsoft Enterprise SOC Lab repository structure.
- Created dedicated folders for Sentinel, Defender, Entra ID, KQL, detection engineering, incident response, threat hunting, attack scenarios, scripts, architecture, and documentation.
- Created phase-based screenshot evidence structure.

### Azure Foundation

- Verified Microsoft Entra tenant.
- Reviewed existing Azure account and subscription state.
- Created dedicated Azure subscription: Microsoft-SOC-Lab.
- Configured Azure cost monitoring.
- Configured INR 500 budget with a 50 percent actual-cost alert.
- Established Azure naming convention.
- Created resource group: rg-microsoft-soc-lab.
- Selected UAE North as the resource group region.
- Applied project governance tags.
- Prepared Log Analytics Workspace: law-microsoft-soc-lab.
- Selected UAE North for the workspace.
- Confirmed Pay-As-You-Go Log Analytics pricing model.

### Evidence

- 01-resource-group-configuration.png
- 02-log-analytics-workspace-configuration.png

### Security

- Billing screenshots excluded from portfolio evidence.
- Sensitive Azure identifiers will not be committed to the repository.
- Added repository rules to prevent common secrets and credentials from being committed.

---

## 2026-10-01

### Microsoft Sentinel and Windows Telemetry

- Enabled Microsoft Sentinel on `law-microsoft-soc-lab`.
- Installed the Windows Security Events solution from Content Hub.
- Configured Windows Security Events via Azure Monitor Agent.
- Connected physical Windows 11 endpoint `LAPTOP-JM2CHLK8` to Azure using Azure Arc.
- Installed and validated the Azure Connected Machine Agent.
- Deployed Azure Monitor Agent to the Arc-connected endpoint.
- Associated the endpoint with the Windows Security Events Data Collection Rule.
- Validated end-to-end Windows Security telemetry ingestion in Microsoft Sentinel.
- Confirmed Event ID 4798 in the Sentinel `SecurityEvent` table.

### Telemetry Optimization

- Replaced broad Windows Security log collection with focused security-event collection.
- Retained security-relevant Event IDs including authentication, privilege, process, account-management, group-membership, discovery, and audit-log events.
- Removed unnecessary broad AppLocker ingestion.
- Reduced telemetry volume to support a cost-conscious lab design.

### KQL Investigation

- Created reusable KQL queries for endpoint behavior.
- Created Windows authentication investigation query.
- Created privileged-logon investigation query.
- Created account/group discovery investigation query.
- Created process-creation investigation query.
- Created multi-event SOC investigation query.
- Created incident timeline reconstruction query.
- Validated KQL investigations against collected Windows telemetry.

### Detection Engineering

Created and validated two Microsoft Sentinel analytics rules:

1. `SOC Lab - Windows Account and Group Discovery`
2. `SOC Lab - Non-SYSTEM Privileged Logon`

Detection engineering work included:

- KQL detection logic
- Severity configuration
- Host entity mapping
- Custom alert details
- Event grouping
- Incident generation
- Noise reduction
- Detection tuning

### Sentinel Incident Investigation

- Account/group discovery detection generated a Microsoft Sentinel incident.
- 42 matching events were grouped into one alert and one incident.
- Investigated host and account context.
- Correlated authentication, privilege, and discovery activity.
- Reconstructed the incident timeline.
- Determined that available evidence did not establish malicious activity.
- Closed the incident with an expected/benign disposition.

### Incident Response

Created:

- `INCIDENT-001-WINDOWS-GROUP-DISCOVERY.md`
- `INCIDENT-001-EVIDENCE-TIMELINE.md`
- `WINDOWS-DISCOVERY-IR-PLAYBOOK.md`
- `07-incident-timeline-reconstruction.kql`

The playbook documents:

- Detection
- Triage
- Investigation
- Containment decision
- Eradication
- Recovery
- Lessons learned
- Detection improvement

### Microsoft Defender for Cloud

- Reviewed Microsoft Defender for Cloud.
- Reviewed Foundational CSPM functionality.
- Reviewed security recommendations.
- Reviewed resource inventory.
- Reviewed security alerts.
- Reviewed Defender plan configuration.
- Kept unnecessary paid Defender plans disabled.

Microsoft Defender for Endpoint and Microsoft Defender XDR were not deployed as part of this lab.

### Microsoft Entra ID Security

- Investigated interactive Entra sign-in activity.
- Investigated an interrupted Azure Portal MFA sign-in.
- Reviewed authentication requirement and error code 50074.
- Reviewed Identity Secure Score.
- Recorded Identity Secure Score of 68.42% during assessment.
- Reviewed identity-security recommendations.
- Reviewed Global Administrator assignment and least-privilege considerations.

### Threat Hunting

Completed three structured threat hunts:

1. Privileged Logon Investigation
2. Account and Group Discovery Investigation
3. Authentication Baseline

Threat-hunting work included:

- Hypothesis development
- Behavioral baselining
- Authentication analysis
- Privileged-session correlation
- Windows Logon ID correlation
- Account/group discovery analysis
- Time-window correlation
- Noise analysis
- Evidence-based analyst conclusions
- Detection-tuning considerations

Created:

- `08-privileged-logon-threat-hunt.kql`
- `09-account-group-discovery-threat-hunt.kql`
- `10-authentication-baseline-threat-hunt.kql`
- `HUNT-01-PRIVILEGED-LOGON.md`
- `HUNT-02-ACCOUNT-GROUP-DISCOVERY.md`
- `HUNT-03-AUTHENTICATION-BASELINE.md`

---

## 2026-10-02

### Final Portfolio Integration

- Rebuilt the main `README.md` to reflect the completed SOC implementation.
- Replaced outdated project-planning information with the actual implemented architecture.
- Removed inaccurate hands-on claims for Microsoft Defender for Endpoint and Microsoft Defender XDR.
- Added completed Azure, Sentinel, telemetry, KQL, detection, incident-response, Entra ID, Defender for Cloud, and threat-hunting sections.
- Added project limitations and scope clarification.
- Added cost-conscious SOC design documentation.
- Added skills-demonstrated section.
- Added final SOC workflow and project conclusion.

### Architecture Documentation

Created:

`architecture/SOC-ARCHITECTURE.md`

Documented the implemented architecture:

`Windows Endpoint -> Azure Arc -> Azure Monitor Agent -> DCR -> Log Analytics -> Microsoft Sentinel`

Documented additional security layers:

- Microsoft Entra ID
- Microsoft Defender for Cloud

Documented:

- Endpoint integration
- Data collection
- SIEM architecture
- Detection pipeline
- Investigation workflow
- Incident response
- Threat hunting
- Cost optimization
- Architecture limitations

### Evidence Index

Created:

`docs/EVIDENCE-INDEX.md`

- Indexed screenshots across project phases.
- Documented what each screenshot demonstrates.
- Recorded 25 screenshots across Phases 01-09.
- Documented why Phase 06 uses investigation artifacts instead of duplicate screenshots.
- Preserved Phase 10 as documentation/finalization rather than manufacturing additional evidence.

### Roadmap Reconciliation

Updated `ROADMAP.md` to reflect the actual completed project.

- Marked implemented phases complete.
- Documented scope changes.
- Separated implemented Defender for Cloud work from unimplemented MDE/XDR functionality.
- Documented Entra ID scope limitations.
- Removed the requirement to generate unnecessary high-volume attack telemetry.
- Aligned the roadmap with the final cost-conscious architecture.

### Portfolio Integrity

- Maintained distinction between implemented and explored technologies.
- Avoided claiming malicious activity where telemetry did not support that conclusion.
- Avoided manufacturing security incidents solely for screenshots.
- Kept sensitive Azure identifiers, credentials, authentication tokens, and billing details out of public documentation.