# Security Controls Assessment

## Project Summary

This lab implemented and tested a **local Windows account lockout policy** on Windows 11 Pro ARM64 running in UTM. The configured values were:

- Threshold: **5** failed logon attempts
- Duration: **15** minutes
- Reset counter: **15** minutes

Testing confirmed that `LabTestUser` was locked after repeated failed sign-ins and that Windows Security logging recorded five Event ID **4625** events and one Event ID **4740** event.

This was a **standalone local configuration**, not an Active Directory or organization-wide deployment.

## ISO/IEC 27001:2022 Annex A Mapping

The table below maps lab activities to **relevant** Annex A themes. Implementing one local setting does **not** satisfy an entire ISO control and does **not** make this project ISO-certified.

| Annex A control | How this lab relates | What this lab does **not** prove |
|---|---|---|
| **A.5.15 Access control** | Lockout restricts continued unauthorized password guessing against a local account after repeated failures. | Organization-wide access control policy, least privilege design, or identity lifecycle management. |
| **A.5.17 Authentication information** | Protects authentication material indirectly by limiting rapid guessing against account passwords. | Password vaulting, secret distribution procedures, or credential lifecycle controls. |
| **A.8.5 Secure authentication** | Adds an authentication protection on the local Windows sign-in path. | MFA, federated identity, passwordless authentication, or hardened authentication architecture. |
| **A.8.15 Logging** | Failed logons and lockouts were recorded in the Windows Security log (4625, 4740). | Centralized log retention, protected log integrity, or SIEM ingestion. |
| **A.8.16 Monitoring activities** | Security events were reviewed after testing to confirm suspicious/failed authentication activity. | Continuous monitoring, alerting, or SOC processes. |
| **A.8.32 Change management** | The change was documented with purpose, impact, validation, and rollback notes before/after testing. | Formal CAB approval, production change windows, or enterprise ticketing evidence. |

## Evidence Reviewed

| Evidence | Finding |
|---|---|
| `Evidence/Configuration/account-lockout-policy-verification.png` | Local Group Policy shows threshold 5 / duration 15 / reset 15 |
| `Evidence/Testing/account-lockout-screen.png` | Sign-in UI reports account currently locked out |
| `Evidence/Testing/failed-logon-events.png` | Five Security events with Event ID 4625 |
| `Evidence/Testing/account-lockout-event-4740.png` | Event Properties confirm Event ID 4740 for `LabTestUser` |

Detailed test outcomes: [Test Plan and Results](../Testing/Test-Plan-and-Results.md)

## Limitations

- Local workstation policy only; not domain Group Policy.
- Single VM lab environment; no production users.
- No multi-factor authentication was configured.
- No centralized logging or continuous monitoring platform was used.
- No third-party audit or ISO certification activity was performed.
- Screenshots include lab identifiers such as `LabTestUser` and the guest computer name.

## Recommendations for Future Improvements

1. Extend the control to a Windows domain lab with Group Policy and a test OU.
2. Add multi-factor authentication for interactive sign-in where supported.
3. Forward Security logs to a central collector or SIEM and create a simple failed-logon alert.
4. Document an account unlock procedure and privileged access path for recovery.
5. Combine lockout with a stronger password policy and attack-surface hardening baseline.
6. Practice a fuller change-management workflow with peer review and rollback rehearsal.

## Conclusion

The lab successfully demonstrates beginner-level security configuration, testing, evidence collection, and awareness of selected ISO/IEC 27001:2022 Annex A controls. It is suitable as a portfolio example of practical Windows security work, with clear boundaries about what was and was not in scope.
