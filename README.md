# Microsoft Enterprise SOC Lab

A hands-on Microsoft security operations lab designed to simulate core enterprise SOC workflows using Microsoft Azure, Microsoft Sentinel, Microsoft Entra ID, Microsoft Defender for Cloud, Windows Security telemetry, and Kusto Query Language (KQL).

The project demonstrates an end-to-end SOC workflow: onboarding a Windows endpoint, collecting focused security telemetry, investigating events with KQL, engineering detections, generating and triaging Microsoft Sentinel incidents, documenting incident-response procedures, investigating identity activity, assessing cloud security posture, and conducting structured threat hunts.

> **Project Status:** Technical implementation completed through Phase 09. Final portfolio integration and documentation are in progress.

---

## Project Objectives

This lab was built to develop practical hands-on experience with:

- Microsoft Sentinel
- Microsoft Entra ID
- Microsoft Defender for Cloud
- Azure Arc
- Azure Monitor Agent (AMA)
- Log Analytics
- Data Collection Rules (DCR)
- Windows Security Events
- Kusto Query Language (KQL)
- Security monitoring and alert triage
- Detection engineering
- Incident investigation and response
- Threat hunting
- Identity security
- Cloud security posture management
- MITRE ATT&CK-informed analysis
- Azure cost governance

---

## High-Level Architecture

    Windows 11 SOC Endpoint
    LAPTOP-JM2CHLK8
            |
            v
        Azure Arc
            |
            v
    Azure Monitor Agent
            |
            v
    Focused Data Collection Rule
            |
            v
    Log Analytics Workspace
    law-microsoft-soc-lab
            |
            v
    Microsoft Sentinel
            |
            +--> KQL Investigation
            +--> Detection Engineering
            +--> Alerts and Incidents
            +--> Incident Investigation
            +--> Incident Response
            +--> Threat Hunting

    Additional Security Layers
            |
            +--> Microsoft Defender for Cloud
            |       |
            |       +--> Foundational CSPM
            |       +--> Security Recommendations
            |       +--> Resource Inventory
            |       +--> Security Alerts Review
            |
            +--> Microsoft Entra ID
                    |
                    +--> Sign-in Investigation
                    +--> MFA Analysis
                    +--> Identity Secure Score
                    +--> Privileged Role Review

---

## SOC Data Pipeline

The primary Windows security telemetry pipeline implemented in the lab is:

    Windows Security Events
            |
            v
    Azure Monitor Agent
            |
            v
    Data Collection Rule
            |
            v
    Log Analytics Workspace
            |
            v
    SecurityEvent Table
            |
            v
           KQL
            |
            +--> Investigation
            +--> Threat Hunting
            +--> Analytics Rules
                        |
                        v
                      Alert
                        |
                        v
                     Incident
                        |
                        v
              Analyst Investigation
                        |
                        v
                  SOC Response

This demonstrates how endpoint telemetry can move through Microsoft's monitoring stack and become usable for investigation, detection, incident triage, and threat hunting.

---

## Azure Environment

| Component | Configuration |
|---|---|
| Azure Subscription | `Microsoft-SOC-Lab` |
| Resource Group | `rg-microsoft-soc-lab` |
| Azure Region | `UAE North` |
| Log Analytics Workspace | `law-microsoft-soc-lab` |
| SOC Endpoint | `LAPTOP-JM2CHLK8` |
| Endpoint OS | Windows 11 |
| Endpoint Integration | Azure Arc |
| Monitoring Agent | Azure Monitor Agent |
| SIEM | Microsoft Sentinel |

### Resource Tags

| Tag | Value |
|---|---|
| Project | `Microsoft-Enterprise-SOC-Lab` |
| Environment | `Lab` |
| Purpose | `Security-Operations` |
| ManagedBy | `Faraz-Shaikh` |

---
## Project Implementation

### Phase 01 - Azure SOC Foundation

Established the Azure foundation required for the SOC environment.

**Implemented:**

- Dedicated `Microsoft-SOC-Lab` Azure subscription
- Resource group `rg-microsoft-soc-lab`
- UAE North deployment region
- Log Analytics Workspace `law-microsoft-soc-lab`
- Resource tagging strategy
- Azure budget and cost monitoring controls

---

### Phase 02 - Microsoft Sentinel

Microsoft Sentinel was enabled on the Log Analytics Workspace to provide SIEM capabilities.

**Implemented:**

- Microsoft Sentinel workspace integration
- Windows Security Events solution from Content Hub
- Windows Security Events via AMA connector
- Data Collection Rule configuration
- Sentinel log ingestion validation

This established the central SIEM platform used throughout the project.

---

### Phase 03 - Windows Endpoint Telemetry

A physical Windows 11 endpoint was integrated with Azure using Azure Arc.

**Endpoint:**

`LAPTOP-JM2CHLK8`

**Telemetry path:**

    Windows 11
        |
        v
    Azure Arc
        |
        v
    Azure Monitor Agent
        |
        v
    Data Collection Rule
        |
        v
    Log Analytics
        |
        v
    Microsoft Sentinel

The Azure Connected Machine Agent was installed and the endpoint successfully registered with Azure Arc.

Azure Monitor Agent was deployed to collect Windows Security telemetry.

End-to-end ingestion was validated by locating Windows Security Event ID `4798` in the Sentinel `SecurityEvent` table.

---

### Focused Data Collection Strategy

Instead of collecting the complete Windows Security log, the DCR was tuned to collect a focused set of security-relevant events:

`1102, 4624, 4625, 4672, 4688, 4720, 4722, 4725, 4726, 4728, 4732, 4756, 4798, 4799`

Examples include:

| Event ID | Security Context |
|---|---|
| 1102 | Security audit log cleared |
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4672 | Special privileges assigned to new logon |
| 4688 | Process creation |
| 4720 | User account created |
| 4726 | User account deleted |
| 4728 / 4732 / 4756 | Group membership changes |
| 4798 | User's local group membership enumerated |
| 4799 | Security-enabled local group membership enumerated |

This reduced unnecessary ingestion while retaining telemetry useful for SOC investigations.

---

### Phase 04 - KQL Investigation

A reusable KQL investigation library was created for Windows security telemetry.

The project contains queries for:

1. Endpoint SecurityEvent behavior
2. Windows authentication investigation
3. Privileged logon investigation
4. Account and group discovery investigation
5. Process creation investigation
6. Multi-event SOC investigation
7. Incident timeline reconstruction
8. Privileged logon threat hunting
9. Account/group discovery threat hunting
10. Authentication baseline threat hunting

Example multi-event investigation:

    SecurityEvent
    | where TimeGenerated > ago(24h)
    | where Computer =~ "LAPTOP-JM2CHLK8"
    | where EventID in (4624, 4625, 4672, 4688, 4798, 4799)
    | extend EventCategory = case(
        EventID == 4624, "Successful Logon",
        EventID == 4625, "Failed Logon",
        EventID == 4672, "Privileged Logon",
        EventID == 4688, "Process Creation",
        EventID == 4798, "User Group Discovery",
        EventID == 4799, "Local Group Discovery",
        "Other"
    )
    | project TimeGenerated, EventCategory, EventID, Account, Activity
    | order by TimeGenerated desc
    | take 100

The queries were used for both reactive investigation and proactive threat hunting.

---

## Detection Engineering

Two Microsoft Sentinel analytics rules were engineered and validated.

### Detection 01 - Windows Account and Group Discovery

**Rule:**

`SOC Lab - Windows Account and Group Discovery`

The rule monitors Windows Security Events `4798` and `4799` for account and local-group discovery activity on the SOC endpoint.

**Severity:** Medium

**Entity Mapping:** Host

**Detection behavior:**

    SecurityEvent 4798 / 4799
            |
            v
    Sentinel Analytics Rule
            |
            v
          Alert
            |
            v
         Incident

The rule successfully generated an incident containing:

- **42 events**
- **1 alert**
- **1 Microsoft Sentinel incident**

This incident was then used for the project's incident-triage workflow.

### Detection 02 - Non-SYSTEM Privileged Logon

**Rule:**

`SOC Lab - Non-SYSTEM Privileged Logon`

The rule monitors Event ID `4672` while excluding routine `NT AUTHORITY\SYSTEM` activity.

This demonstrates detection tuning by removing a high-volume expected account while retaining potentially more interesting privileged logon activity.

---

## Incident Investigation

A Microsoft Sentinel incident generated by the account/group discovery detection was investigated.

The investigation included:

- Alert validation
- Host identification
- Event inspection
- Account analysis
- Timeline reconstruction
- Authentication correlation
- Privileged-logon correlation
- Discovery-event analysis
- Analyst disposition

The incident contained 42 matching events grouped into one alert and one incident.

The activity was investigated rather than automatically classified as malicious.

Available telemetry did not establish malicious activity, and the incident was closed as expected/benign activity after analysis.

This demonstrates an important SOC principle:

> A detection is an investigative lead, not automatic proof of compromise.

---

## Incident Response

A Windows discovery incident-response playbook was created covering:

1. Detection
2. Triage
3. Investigation
4. Containment decision
5. Eradication
6. Recovery
7. Lessons learned
8. Detection improvement

Containment actions were intentionally conditional. Because the investigated incident did not establish malicious activity, destructive or unnecessary containment actions were not performed.

An evidence timeline was also created to document the investigation chronologically.

---
## Microsoft Defender for Cloud

### Phase 07 - Cloud Security Posture Assessment

Microsoft Defender for Cloud was reviewed to understand cloud security posture, recommendations, inventory, and security alerts.

The lab used the available **Foundational CSPM** capabilities while intentionally avoiding unnecessary paid Defender plans.

Reviewed areas included:

- Security recommendations
- Resource inventory
- Security alerts
- Cloud security posture
- Defender plan configuration

At the time of assessment:

- 1 Azure environment was visible
- No Critical, High, Medium, or Low recommendations were displayed
- No attack paths were displayed
- No active security alerts were present

Paid Defender plans were intentionally left disabled to maintain the project's minimal-cost design.

### Defender Scope Clarification

This project demonstrates hands-on use of **Microsoft Defender for Cloud**.

Microsoft Defender for Endpoint and Microsoft Defender XDR were **not deployed as part of this lab**. Access to the Defender XDR portal was unavailable in the lab tenant, so the project does not claim hands-on MDE/XDR implementation.

This distinction is intentionally documented to keep the portfolio technically accurate.

---

## Microsoft Entra ID Security

### Phase 08 - Identity Investigation

Microsoft Entra ID was used to investigate identity-related security information including:

- Interactive sign-ins
- Authentication status
- MFA requirements
- Identity Secure Score
- Administrative role assignments
- Identity security recommendations

### MFA Sign-In Investigation

An interrupted Azure Portal sign-in was investigated.

Observed information included:

- Application: Azure Portal
- Status: Interrupted
- Error code: `50074`
- Authentication requirement: MFA

The Entra sign-in details indicated that additional multi-factor authentication was required.

The event was investigated in context rather than being automatically treated as malicious.

This reinforced another SOC principle:

> A failed or interrupted authentication event does not automatically mean an attack occurred.

---

### Identity Secure Score

The lab tenant showed an Identity Secure Score of:

**68.42%**

Security recommendations reviewed included:

- Use least privileged administrative roles
- Designate more than one Global Administrator
- Password hash synchronization
- Password expiration configuration
- User consent for applications

The tenant contained a single Global Administrator assignment.

Because this was a single-user lab environment, administrative-role changes that could cause account lockout were not performed solely for demonstration purposes.

---

## Threat Hunting

### Phase 09 - Structured Threat Hunting

Three threat-hunting investigations were performed using existing Sentinel telemetry.

The methodology followed:

    Hypothesis
        |
        v
    Identify Relevant Telemetry
        |
        v
    Establish Baseline
        |
        v
    Identify Interesting Activity
        |
        v
    Correlate Events
        |
        v
    Review Evidence
        |
        v
    Analyst Conclusion
        |
        v
    Detection Improvement Opportunity

---

### Hunt 01 - Privileged Logon Investigation

**Hypothesis:** Non-SYSTEM privileged logons may identify interactive administrative activity requiring investigation.

A seven-day baseline of Event ID `4672` showed:

| Account | Privileged Logons |
|---|---:|
| `NT AUTHORITY\SYSTEM` | 132 |
| `LAPTOP-JM2CHLK8\Faraz Shaikh` | 4 |

A local-user privileged session was correlated with Event ID `4624`.

The investigation identified:

- Event ID `4624` - Successful logon
- Logon Type `2` - Interactive
- Event ID `4672` - Special privileges assigned
- Matching Windows Logon ID `0x1080b537`

Matching logon identifiers demonstrated that the authentication event and privileged-logon event belonged to the same Windows session.

No evidence from the investigated telemetry established malicious activity.

---

### Hunt 02 - Account and Group Discovery

**Hypothesis:** High-volume account/group enumeration may identify discovery activity requiring further investigation.

A seven-day baseline of Events `4798` and `4799` identified substantial enumeration activity.

Observed counts included:

| Account / Event | Count |
|---|---:|
| Machine account - 4798 | 6,075 |
| Local user - 4798 | 71 |
| Machine account - 4799 | 16 |
| Local user - 4799 | 9 |

A notable burst of Event ID `4798` was identified.

During one selected 30-minute investigation window:

- Machine-account 4798 events: **570**
- SYSTEM successful logons: **3**
- SYSTEM privileged logons: **3**
- Local-user 4798 events: **2**

No failed logons, process-creation events, or 4799 events were returned by the selected correlation query during that window.

The available telemetry therefore demonstrated unusual/high-volume discovery behavior but did not establish a malicious cause.

This hunt highlighted the importance of baselining, contextual investigation, and detection tuning.

---

### Hunt 03 - Authentication Baseline

**Hypothesis:** Establishing normal authentication behavior can make future anomalies easier to identify.

A seven-day baseline of Events `4624` and `4625` showed:

| Account | Result | Logon Type | Count |
|---|---|---|---:|
| `NT AUTHORITY\SYSTEM` | Successful | 5 - Service | 132 |
| `LAPTOP-JM2CHLK8\Faraz Shaikh` | Successful | 2 - Interactive | 8 |

No Event ID `4625` records were returned by the query for the investigated period.

This does not prove that failed authentication never occurred; it means none were present in the telemetry returned by the scoped hunt.

No authentication anomaly requiring escalation was identified.

---

## Threat Hunting Outcomes

The three hunts demonstrated practical experience with:

- Hypothesis-driven hunting
- Windows authentication analysis
- Privileged-session correlation
- Logon ID correlation
- Behavioral baselining
- Discovery-event investigation
- Noise identification
- KQL aggregation
- Time-window correlation
- Evidence-based analyst conclusions
- Detection tuning

Most importantly, the hunts avoided treating every unusual event as malicious without supporting evidence.

---
## Project Phase Summary

| Phase | Area | Status |
|---|---|---|
| 01 | Azure SOC Foundation | Complete |
| 02 | Microsoft Sentinel | Complete |
| 03 | Azure Arc / AMA / Windows Telemetry | Complete |
| 04 | KQL Investigation | Complete |
| 05 | Detection Engineering | Complete |
| 06 | Incident Response | Complete |
| 07 | Microsoft Defender for Cloud | Complete |
| 08 | Microsoft Entra ID Security | Complete |
| 09 | Threat Hunting | Complete |
| 10 | Final Portfolio Integration | In Progress |

---

## KQL Library

The repository contains ten reusable KQL investigation and threat-hunting queries:

1. `01-endpoint-securityevent-behavior.kql`
2. `02-windows-authentication-investigation.kql`
3. `03-privileged-logon-investigation.kql`
4. `04-account-group-discovery-investigation.kql`
5. `05-process-creation-investigation.kql`
6. `06-multi-event-soc-investigation.kql`
7. `07-incident-timeline-reconstruction.kql`
8. `08-privileged-logon-threat-hunt.kql`
9. `09-account-group-discovery-threat-hunt.kql`
10. `10-authentication-baseline-threat-hunt.kql`

These queries cover authentication, privilege activity, process activity, discovery behavior, incident reconstruction, correlation, and behavioral baselining.

---

## Detection Rules

Detection engineering documentation is available under:

`detection-rules/`

Implemented rules:

- `01-windows-account-group-discovery.md`
- `02-non-system-privileged-logon.md`

The rules demonstrate:

- KQL-based detection logic
- Severity configuration
- Entity mapping
- Custom alert details
- Event grouping
- Incident generation
- Noise reduction
- Detection tuning

---

## Incident Response Artifacts

The `incident-response/` directory contains:

- `INCIDENT-001-WINDOWS-GROUP-DISCOVERY.md`
- `INCIDENT-001-EVIDENCE-TIMELINE.md`
- `WINDOWS-DISCOVERY-IR-PLAYBOOK.md`

These artifacts document both the technical investigation and the analyst decision-making process.

---

## Threat Hunting Reports

The `threat-hunting/` directory contains:

- `HUNT-01-PRIVILEGED-LOGON.md`
- `HUNT-02-ACCOUNT-GROUP-DISCOVERY.md`
- `HUNT-03-AUTHENTICATION-BASELINE.md`

Each hunt documents the hypothesis, query approach, evidence, findings, and analyst conclusion.

---

## Evidence and Screenshots

Configuration and investigation evidence is maintained under the `screenshots/` directory and organized by project phase.

Evidence includes:

- Azure resource configuration
- Log Analytics deployment
- Microsoft Sentinel configuration
- Windows Security Events connector
- Data Collection Rule configuration
- Azure Arc endpoint connection
- Telemetry ingestion validation
- KQL investigations
- Sentinel analytics-rule validation
- Sentinel incident creation
- Incident triage and closure
- Defender for Cloud assessment
- Entra ID sign-in investigation
- Identity Secure Score review
- Privileged-logon correlation
- Account/group discovery hunting
- Authentication baselining

Screenshots are retained as evidence of hands-on implementation rather than being used as a substitute for technical documentation.

---

## Cost-Conscious SOC Design

The lab was intentionally designed to minimize unnecessary Azure consumption.

Cost controls included:

- Azure budget monitoring
- Budget alerts
- Use of an existing physical Windows endpoint
- Azure Arc instead of deploying an additional Windows Azure VM
- Focused Windows Security Event collection
- Removal of broad `Security!*` ingestion
- Avoidance of unnecessary AppLocker log ingestion
- Avoidance of paid Microsoft Defender plans where they were not required
- Reuse of existing telemetry for threat hunting
- Controlled generation of security events

The Azure budget is an alerting mechanism and not a hard spending limit.

---

## Skills Demonstrated

This project provides hands-on evidence of experience with:

**SIEM and Security Monitoring**

- Microsoft Sentinel
- Log Analytics
- Windows Security Events
- SecurityEvent table analysis
- Alert and incident workflows

**KQL and Threat Detection**

- Kusto Query Language
- Detection logic development
- Event filtering
- Aggregation
- Time-window analysis
- Event correlation
- Behavioral baselining
- Detection tuning

**Endpoint Telemetry**

- Azure Arc
- Azure Monitor Agent
- Data Collection Rules
- Windows authentication events
- Windows privilege events
- Account/group discovery events

**Incident Response**

- Alert triage
- Incident investigation
- Evidence collection
- Timeline reconstruction
- Analyst disposition
- Response playbook development

**Threat Hunting**

- Hypothesis-driven hunting
- Authentication baselining
- Privileged-session investigation
- Account/group discovery analysis
- Noise reduction
- Evidence-based conclusions

**Identity Security**

- Microsoft Entra ID
- Sign-in log investigation
- MFA analysis
- Identity Secure Score
- Privileged-role review

**Cloud Security**

- Microsoft Defender for Cloud
- Foundational CSPM
- Security recommendations
- Security alerts review
- Azure cost governance

---

## Project Limitations

This is a controlled portfolio lab rather than a production SOC environment.

Important limitations include:

- Single primary Windows endpoint
- Limited telemetry volume
- Short observation windows
- No production user population
- No production Active Directory domain
- Microsoft Defender for Endpoint was not deployed
- Microsoft Defender XDR was not implemented
- Paid Defender for Cloud plans were intentionally not enabled
- Some event types may not appear if the relevant activity did not occur during the collection period

These limitations are documented intentionally so that project claims remain aligned with the environment that was actually implemented.

---

## Key SOC Lessons

Several operational principles were reinforced throughout the project:

1. **An event is not automatically an alert.**
2. **An alert is not automatically an incident.**
3. **An incident is not automatically a confirmed compromise.**
4. **Successful authentication is not automatically safe.**
5. **Failed or interrupted authentication is not automatically malicious.**
6. **High-volume activity requires context and baselining.**
7. **Detection rules require tuning to reduce expected noise.**
8. **Correlation provides stronger context than isolated events.**
9. **Containment should be based on evidence and impact.**
10. **Analyst conclusions should reflect what the available telemetry actually supports.**

---

## Repository Structure

    Microsoft-Enterprise-SOC-Lab/
    |
    +-- defender/
    +-- detection-rules/
    +-- docs/
    +-- entra-id/
    +-- incident-response/
    +-- kql/
    +-- screenshots/
    +-- threat-hunting/
    |
    +-- README.md
    +-- ROADMAP.md
    +-- CHANGELOG.md

Additional directories are retained for future expansion of the lab.

---

## Security Notice

Credentials, secrets, billing information, subscription identifiers, tenant identifiers, authentication tokens, and other sensitive account information are intentionally excluded from this repository.

The project documentation focuses on architecture, security telemetry, detection logic, investigation methodology, and SOC workflows.

---

## Conclusion

The Microsoft Enterprise SOC Lab demonstrates a practical Microsoft security operations workflow from endpoint telemetry collection through investigation and response.

The project integrates:

**Azure Arc → Azure Monitor Agent → Data Collection Rule → Log Analytics → Microsoft Sentinel → KQL → Detection → Alert → Incident → Investigation → Response → Threat Hunting**

It also extends the SOC workflow into identity and cloud-security analysis through Microsoft Entra ID and Microsoft Defender for Cloud.

The primary goal of the project is not simply to deploy Microsoft security products, but to demonstrate the analyst workflow required to understand telemetry, investigate security activity, tune detections, correlate evidence, document incidents, and make defensible security decisions.