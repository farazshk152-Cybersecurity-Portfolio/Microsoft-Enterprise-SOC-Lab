# Microsoft Enterprise SOC Lab - Roadmap

## Phase 0 - Environment Readiness
- [x] Verify Microsoft Entra tenant
- [x] Configure Azure subscription
- [x] Configure cost monitoring
- [x] Establish project structure

## Phase 1 - Azure SOC Foundation
- [x] Create resource group
- [x] Define tagging strategy
- [ ] Deploy Log Analytics Workspace
- [ ] Enable Microsoft Sentinel

## Phase 2 - Microsoft Sentinel
- [ ] Explore Sentinel architecture
- [ ] Configure Sentinel
- [ ] Review Content Hub
- [ ] Configure required data connectors
- [ ] Understand Sentinel tables and data flow

## Phase 3 - Security Telemetry
- [ ] Connect Windows security telemetry
- [ ] Configure data collection
- [ ] Generate controlled security events
- [ ] Verify ingestion
- [ ] Understand important Sentinel tables

## Phase 4 - KQL
- [ ] Learn KQL fundamentals
- [ ] Filtering and searching
- [ ] summarize
- [ ] project
- [ ] extend
- [ ] joins
- [ ] time-based analysis
- [ ] Build SOC investigation queries

## Phase 5 - Detection Engineering
- [ ] Create analytics rules
- [ ] Map detections to MITRE ATT&CK
- [ ] Generate controlled attack activity
- [ ] Validate detections
- [ ] Tune false positives

## Phase 6 - Incident Response
- [ ] Generate Sentinel incidents
- [ ] Perform L1 triage
- [ ] Investigate entities and evidence
- [ ] Determine severity and scope
- [ ] Document escalation
- [ ] Perform L2 investigation workflow

## Phase 7 - Microsoft Defender
- [ ] Explore Defender XDR
- [ ] Defender for Endpoint onboarding where licensing permits
- [ ] Endpoint investigation
- [ ] Device timeline
- [ ] Alerts and incidents
- [ ] Advanced Hunting

## Phase 8 - Microsoft Entra ID
- [ ] Create lab identities
- [ ] Create security groups
- [ ] Explore roles and RBAC
- [ ] Analyze sign-in and audit activity
- [ ] Investigate identity security events
- [ ] Study Conditional Access where licensing permits

## Phase 9 - Threat Hunting
- [ ] Build hunting hypotheses
- [ ] Perform KQL-based hunts
- [ ] Map activity to MITRE ATT&CK
- [ ] Document findings
- [ ] Convert useful hunts into detections

## Phase 10 - Final Enterprise Investigation
- [ ] Generate multi-stage attack scenario
- [ ] Detect suspicious activity
- [ ] Triage alerts
- [ ] Investigate incident
- [ ] Hunt for related activity
- [ ] Determine scope
- [ ] Document containment/remediation
- [ ] Produce final incident report
- [ ] Finalize GitHub documentation
- [ ] Convert project experience into resume/interview material
