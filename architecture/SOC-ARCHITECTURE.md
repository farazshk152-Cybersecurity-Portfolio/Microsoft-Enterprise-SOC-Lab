# Microsoft Enterprise SOC Lab - Architecture

## Overview

The Microsoft Enterprise SOC Lab implements a cost-conscious security monitoring architecture using Microsoft Azure and a physical Windows 11 endpoint.

The environment was designed to demonstrate the flow of security telemetry from an endpoint into Microsoft Sentinel and then through SOC investigation, detection engineering, incident response, and threat-hunting workflows.

---

## Architecture Diagram

    +-------------------------------+
    | Windows 11 SOC Endpoint       |
    | LAPTOP-JM2CHLK8               |
    +---------------+---------------+
                    |
                    | Azure Arc
                    v
    +-------------------------------+
    | Azure Connected Machine       |
    +---------------+---------------+
                    |
                    | Azure Monitor Agent
                    v
    +-------------------------------+
    | Data Collection Rule          |
    | Focused Security Events       |
    +---------------+---------------+
                    |
                    v
    +-------------------------------+
    | Log Analytics Workspace       |
    | law-microsoft-soc-lab         |
    +---------------+---------------+
                    |
                    v
    +-------------------------------+
    | Microsoft Sentinel            |
    +---------------+---------------+
                    |
          +---------+---------+
          |         |         |
          v         v         v
        KQL      Analytics   Threat
    Investigation  Rules     Hunting
                    |
                    v
                  Alert
                    |
                    v
                 Incident
                    |
                    v
              SOC Triage
                    |
                    v
             Investigation
                    |
                    v
           Incident Response


    Additional Security Layers

    +-----------------------------+
    | Microsoft Entra ID          |
    +-----------------------------+
    | Sign-in Investigation       |
    | MFA Analysis                |
    | Identity Secure Score       |
    | Privileged Role Review      |
    +-----------------------------+

    +-----------------------------+
    | Microsoft Defender for Cloud|
    +-----------------------------+
    | Foundational CSPM           |
    | Recommendations Review      |
    | Resource Inventory          |
    | Security Alerts Review      |
    +-----------------------------+

---

## Azure Foundation

The lab uses the following Azure resources:

| Component | Configuration |
|---|---|
| Subscription | Microsoft-SOC-Lab |
| Resource Group | rg-microsoft-soc-lab |
| Region | UAE North |
| Log Analytics Workspace | law-microsoft-soc-lab |
| SIEM | Microsoft Sentinel |
| Endpoint | LAPTOP-JM2CHLK8 |
| Endpoint OS | Windows 11 |

---

## Endpoint Integration

The Windows 11 endpoint was connected to Azure using Azure Arc.

Azure Arc allows the physical endpoint to be represented and managed as an Azure-connected machine without requiring the endpoint itself to be an Azure virtual machine.

The Azure Monitor Agent was deployed to the Arc-connected endpoint.

Telemetry flow:

    Windows Security Log
            |
            v
    Azure Monitor Agent
            |
            v
    Data Collection Rule
            |
            v
    Log Analytics Workspace

---

## Data Collection

A focused Data Collection Rule was used instead of collecting the complete Windows Security log.

Security events collected included:

- 1102 - Security audit log cleared
- 4624 - Successful logon
- 4625 - Failed logon
- 4672 - Special privileges assigned
- 4688 - Process creation
- 4720 - User account created
- 4722 - User account enabled
- 4725 - User account disabled
- 4726 - User account deleted
- 4728 - Member added to global security group
- 4732 - Member added to local security group
- 4756 - Member added to universal security group
- 4798 - User local-group membership enumerated
- 4799 - Security-enabled local-group membership enumerated

The focused collection strategy reduces unnecessary ingestion while retaining events useful for SOC monitoring and investigation.

---

## SIEM Layer

Microsoft Sentinel provides the central SIEM layer.

Telemetry collected in Log Analytics is queried through the `SecurityEvent` table using Kusto Query Language.

Sentinel was used for:

- Security-event investigation
- KQL analysis
- Analytics rules
- Alert generation
- Incident creation
- Incident triage
- Timeline reconstruction
- Threat hunting

---

## Detection Pipeline

The implemented detection workflow is:

    SecurityEvent
          |
          v
    KQL Detection Logic
          |
          v
    Sentinel Analytics Rule
          |
          v
         Alert
          |
          v
        Incident
          |
          v
    Analyst Triage
          |
          v
    Investigation
          |
          v
    Response Decision

Two analytics rules were implemented:

1. Windows Account and Group Discovery
2. Non-SYSTEM Privileged Logon

---

## Investigation and Response

The SOC investigation workflow combines multiple event types rather than relying on isolated alerts.

Relevant telemetry includes:

- Authentication events
- Privileged logons
- Process creation
- Account changes
- Group membership changes
- Account/group discovery activity

The analyst workflow follows:

    Detection
       |
       v
    Triage
       |
       v
    Validate Alert
       |
       v
    Identify Host / Account
       |
       v
    Correlate Events
       |
       v
    Build Timeline
       |
       v
    Determine Scope
       |
       v
    Response Decision
       |
       +--> Benign / Expected -> Close and Tune
       |
       +--> Suspicious -> Continue Investigation
       |
       +--> Confirmed Threat -> Containment / Eradication / Recovery

---

## Identity Security Layer

Microsoft Entra ID provides identity-security visibility.

The project included:

- Interactive sign-in investigation
- MFA requirement analysis
- Authentication-status analysis
- Identity Secure Score review
- Administrative-role review

Identity telemetry was analyzed separately from the Windows `SecurityEvent` pipeline but forms part of the overall SOC investigation model.

---

## Cloud Security Layer

Microsoft Defender for Cloud was used for cloud-security posture assessment.

The project reviewed:

- Foundational CSPM
- Security recommendations
- Resource inventory
- Security alerts
- Defender plan configuration

Paid Defender plans were intentionally left disabled to preserve the minimal-cost design.

Microsoft Defender for Endpoint and Microsoft Defender XDR were not deployed in this lab.

---

## Threat Hunting Layer

Microsoft Sentinel telemetry was used for hypothesis-driven threat hunting.

Three hunts were completed:

1. Privileged Logon Investigation
2. Account and Group Discovery Investigation
3. Authentication Baseline

The hunting workflow follows:

    Hypothesis
        |
        v
    Select Telemetry
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
    Detection Improvement

---

## Cost Optimization

The architecture was intentionally designed for low-cost portfolio use.

Cost-control decisions included:

- Physical Windows endpoint instead of an additional Azure VM
- Azure Arc integration
- Focused Data Collection Rule
- Controlled Windows event ingestion
- Azure budget monitoring
- Avoidance of unnecessary paid Defender plans
- Reuse of existing telemetry for investigations and threat hunting

---

## Architecture Limitations

This is a controlled SOC lab rather than a production enterprise deployment.

Current limitations include:

- Single primary Windows endpoint
- No production Active Directory domain
- No large user population
- Limited telemetry volume
- No Microsoft Defender for Endpoint deployment
- No Microsoft Defender XDR deployment
- No paid Defender for Cloud plans
- Short observation periods

These limitations are documented so that the architecture accurately represents what was implemented.

---

## Summary

The core SOC architecture implemented in this project is:

    Windows Endpoint
          |
          v
      Azure Arc
          |
          v
        AMA
          |
          v
         DCR
          |
          v
    Log Analytics
          |
          v
    Microsoft Sentinel
          |
          +--> KQL
          +--> Detection Engineering
          +--> Alerts
          +--> Incidents
          +--> Incident Response
          +--> Threat Hunting

Microsoft Entra ID and Microsoft Defender for Cloud extend the architecture with identity-security and cloud-security-posture visibility.