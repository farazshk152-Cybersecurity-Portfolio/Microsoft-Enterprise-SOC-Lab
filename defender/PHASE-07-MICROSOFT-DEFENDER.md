# Phase 07 - Microsoft Defender for Cloud

## Objective

Evaluate Microsoft Defender capabilities available within the Microsoft Enterprise SOC Lab while maintaining a cost-controlled Azure environment.

## Environment

- Azure Subscription: Microsoft-SOC-Lab
- Resource Group: rg-microsoft-soc-lab
- Log Analytics Workspace: law-microsoft-soc-lab
- SOC Endpoint: LAPTOP-JM2CHLK8
- Endpoint connectivity: Azure Arc
- SIEM: Microsoft Sentinel

## Defender Capability Assessment

Microsoft Defender for Cloud was available for the lab subscription.

The Microsoft Defender XDR portal could not be used with the current tenant/account configuration, so Defender for Endpoint/XDR functionality was not represented as implemented in this project.

The lab therefore focused on Microsoft Defender for Cloud capabilities that were actually available.

## Defender Plans Review

The Defender for Cloud subscription configuration was reviewed before enabling additional services.

Observed configuration:

- Foundational CSPM: Available with free monitoring coverage
- Defender CSPM: Disabled
- Defender for Servers: Disabled
- Defender for App Service: Disabled
- Defender for Databases: Disabled
- Defender for Storage: Disabled
- Defender for Containers: Disabled
- Other paid workload protection plans: Disabled

Paid Defender plans were intentionally not enabled in order to keep the SOC lab cost controlled.

## Cloud Security Posture Management

Microsoft Defender for Cloud Foundational CSPM was reviewed.

The following security posture capabilities were explored:

- Security recommendations
- Resource inventory
- Security alerts
- Defender plan coverage
- Cloud security posture concepts

At the time of assessment, no active security recommendations requiring remediation were displayed.

## Asset Inventory

Defender for Cloud Inventory was reviewed.

The Log Analytics workspace:

law-microsoft-soc-lab

was visible as an Azure resource.

The Azure Arc connected endpoint was independently verified through Azure Arc and Microsoft Sentinel telemetry during earlier phases of the project.

## Security Alerts

The Microsoft Defender for Cloud Security Alerts interface was reviewed.

At the time of assessment:

- Open alerts: 0
- Active alerts: 0
- In-progress alerts: 0
- Affected resources: 0

No artificial cloud attacks or additional paid workload protection services were enabled solely to generate Defender alerts.

## Security Operations Concepts Learned

This phase demonstrated the distinction between several Microsoft security technologies.

### Microsoft Defender for Cloud

Provides capabilities including:

- Cloud security posture management
- Security recommendations
- Resource inventory
- Workload protection when appropriate Defender plans are enabled
- Cloud security alerts

### Microsoft Defender XDR

Provides extended detection and response capabilities across supported Microsoft security products such as endpoints, identities, email and applications.

### Microsoft Sentinel

Acts as the SIEM and security operations platform used in this lab for:

- Log ingestion
- KQL investigation
- Detection engineering
- Alerting
- Incident creation
- Threat hunting
- Incident investigation

## Cost-Control Decision

The project intentionally avoided enabling paid Defender workload protection plans simply to create additional security findings.

This maintained the lab's objective of demonstrating practical Microsoft security operations while minimizing unnecessary Azure consumption.

## Evidence

Screenshot:

screenshots/phase-07-defender/01-defender-for-cloud-security-alerts.png

## Outcome

Phase 07 provided practical exposure to Microsoft Defender for Cloud, Foundational CSPM, cloud security posture, asset inventory, recommendations, security alert management and Defender plan evaluation.

The phase also demonstrated the importance of validating licensing and available security capabilities before designing or enabling security controls.
