# Phase 08 - Microsoft Entra ID Security

## Objective

Investigate identity authentication activity and evaluate the security posture of the Microsoft Entra ID tenant used by the Microsoft Enterprise SOC Lab.

## Environment

- Identity platform: Microsoft Entra ID
- Directory: Default Directory
- Users: 1
- Administrative role reviewed: Global Administrator
- SIEM: Microsoft Sentinel

## Identity Security Investigation

Microsoft Entra sign-in logs were reviewed for interactive authentication activity.

The logs contained successful Azure Portal and Microsoft Azure CLI sign-ins as well as an interrupted Azure Portal authentication event.

### Investigated Event

The selected event showed:

- Application: Azure Portal
- Status: Interrupted
- Authentication requirement: Multifactor authentication
- Continuous Access Evaluation: No

Microsoft Entra reported that the user needed to perform multifactor authentication.

The event was therefore investigated as an authentication workflow event rather than automatically treating the interrupted status as malicious activity.

## Authentication Details

Authentication Details were reviewed for the event.

Individual authentication steps showed successful results, including a previously satisfied authentication requirement.

This demonstrated that individual authentication stages can succeed while the overall sign-in remains interrupted because an additional authentication requirement still needs to be completed.

## SOC Investigation Principle

Authentication status alone is not sufficient to determine whether an identity is compromised.

An analyst should correlate:

- User
- Application
- Authentication result
- MFA requirements
- Source IP
- Location
- Device information
- Authentication details
- Conditional Access
- Surrounding sign-in activity

Important principle:

Interrupted does not automatically mean compromised.
Failed does not automatically mean malicious.
Successful does not automatically mean safe.

## Audit Logs

Microsoft Entra audit logs were reviewed.

No audit records were available in the displayed log window during the assessment.

No artificial directory changes were generated solely to populate the audit log.

## Identity Secure Score

Microsoft Entra Identity Secure Score was reviewed.

Observed score:

68.42%

Five identity-security recommendations were displayed.

Examples included:

- Use least privileged administrative roles
- Designate more than one global administrator
- Do not allow users to grant consent to unreliable applications

Other displayed controls were already marked completed.

## Privileged Role Review

The Global Administrator role was reviewed through Microsoft Entra Roles and Administrators.

The lab tenant contained one Global Administrator assignment at directory scope.

The assignment was not modified because this is a minimal single-user lab tenant and removing or changing the only administrative assignment could create unnecessary tenant lockout risk.

In a production environment, privileged administrative access should follow least-privilege principles and routine activities should use narrower administrative roles where possible.

## Security Concepts Learned

This phase demonstrated:

- Microsoft Entra ID identity monitoring
- Interactive sign-in investigation
- Multifactor authentication analysis
- Authentication event correlation
- Identity Secure Score
- Identity security recommendations
- Administrative role review
- RBAC
- Least privilege
- Identity security posture assessment

## Evidence

Screenshots:

- screenshots/phase-08-entra/01-entra-interrupted-mfa-signin-investigation.png
- screenshots/phase-08-entra/02-entra-identity-secure-score.png

## Outcome

Phase 08 added identity-security investigation and security-posture analysis to the Microsoft Enterprise SOC Lab.

The phase demonstrated how SOC analysts can investigate authentication activity while security engineers evaluate identity configuration, privileged access and recommended security improvements.
