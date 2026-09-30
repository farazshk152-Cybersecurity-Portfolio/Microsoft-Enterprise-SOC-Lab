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
