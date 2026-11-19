# Account Lockout Policy

## Purpose

This policy defines how a local Windows 11 account responds to repeated failed sign-in attempts. The goal is to slow down password guessing and brute-force attempts against local user accounts.

This document describes a **lab configuration** on a single Windows 11 Pro ARM64 virtual machine. It is not an Active Directory domain policy and does not claim enterprise-wide enforcement.

## Scope

| Item | Value |
|---|---|
| System | Windows 11 Pro ARM64 (UTM VM) |
| Policy location | Local Group Policy Editor → Computer Configuration → Windows Settings → Security Settings → Account Policies → Account Lockout Policy |
| Account type | Local accounts |
| Test account | `LabTestUser` (created for validation only) |

## Configured Settings

| Setting | Value | Rationale |
|---|---|---|
| Account lockout threshold | **5** invalid logon attempts | Allows a small number of typos while limiting sustained guessing |
| Account lockout duration | **15** minutes | Temporary lockout reduces disruption while still blocking rapid retries |
| Reset account lockout counter after | **15** minutes | Aligns the observation window with the lockout duration |
| Allow Administrator account lockout | Enabled (as shown in lab evidence) | Consistent lockout behavior for accounts covered by local policy |

## Security Benefits

- Reduces the effectiveness of simple password spraying or repeated guessing against a local account.
- Creates clear Security log evidence (failed logons and lockout events) that can be reviewed after an incident.
- Supports access-control and authentication hygiene practices described in ISO/IEC 27001:2022 Annex A controls such as A.5.15, A.5.17, and A.8.5 (see project README and final assessment for mapping notes).

## Limitations

- Applies only to this local Windows configuration; it does not protect domain accounts or cloud identities.
- A determined attacker with physical or administrative access can still bypass or change local policy.
- Lockout can be used for denial-of-service against a known username if threshold and duration are too aggressive.
- Does not replace strong passwords, multi-factor authentication, patching, or centralized identity controls.
- This lab does not demonstrate ISO certification or full organizational compliance.

## Related Documents

- [Configuration Steps](../Implementation/Configuration-Steps.md)
- [Change Request](../Change-Management/Change-Request.md)
- [Test Plan and Results](../Testing/Test-Plan-and-Results.md)
- [Evidence Index](../Evidence/README.md)
