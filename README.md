# Microsoft Enterprise SOC Lab

A hands-on Microsoft security operations lab built to simulate an enterprise SOC environment using Microsoft Azure and the Microsoft security ecosystem.

## Project Objectives

This project is designed to develop practical experience with:

- Microsoft Sentinel
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Entra ID
- Log Analytics
- Kusto Query Language (KQL)
- Security monitoring and alert triage
- Detection engineering
- Incident investigation and response
- Threat hunting
- Endpoint security
- MITRE ATT&CK mapping
- Azure security and cost governance

## Architecture

The lab uses a dedicated Azure subscription and resource group for Microsoft security resources.

Current foundation:

Microsoft Entra ID  
?  
Microsoft-SOC-Lab Azure Subscription  
?  
rg-microsoft-soc-lab  
?  
law-microsoft-soc-lab  
?  
Microsoft Sentinel

Additional security components will be integrated as the project progresses.

## Current Progress

### Phase 0 - Environment and Azure Readiness

Completed:

- Microsoft Entra tenant verified
- Azure billing/account state reviewed
- Dedicated Microsoft-SOC-Lab subscription created
- Cost budget configured
- Cost alert configured
- Azure resource naming convention established
- Project repository structure created

### Phase 1 - Azure SOC Foundation

Completed:

- Resource group designed and created
- Region selected: UAE North
- Azure resource tagging strategy implemented
- Log Analytics Workspace configuration prepared

In progress:

- Log Analytics Workspace deployment
- Microsoft Sentinel onboarding

## Resource Naming

Subscription:
Microsoft-SOC-Lab

Resource Group:
rg-microsoft-soc-lab

Log Analytics Workspace:
law-microsoft-soc-lab

## Resource Tags

Project: Microsoft-Enterprise-SOC-Lab  
Environment: Lab  
Purpose: Security-Operations  
ManagedBy: Faraz-Shaikh

## Cost Management

This project uses a minimal-cost architecture.

Controls include:

- Azure budget monitoring
- Cost alerting
- Local lab systems where practical
- Controlled log ingestion
- Avoidance of unnecessary Azure infrastructure

The configured Azure budget is an alerting mechanism and is not a hard spending limit.

## Documentation

Each project phase contains:

- Configuration evidence
- Technical explanation
- Commands and queries
- Troubleshooting notes
- Security findings
- SOC investigation workflow
- Interview-relevant learning points

## Security Notice

Credentials, secrets, billing information, subscription identifiers, tenant identifiers, and other sensitive account information are intentionally excluded from this repository.
