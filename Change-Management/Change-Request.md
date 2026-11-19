# Change Request — Local Account Lockout Policy

This is a **lab change record** written in a simple change-management format. It documents what was configured on a personal Windows 11 VM for learning and portfolio purposes. It does not represent an approved enterprise change ticket or production CAB decision.

## Change Summary

| Field | Detail |
|---|---|
| Change title | Enable local Windows account lockout policy |
| Environment | Windows 11 Pro ARM64 guest in UTM on MacBook Pro M2 |
| Change type | Local security configuration |
| Scope | Single standalone workstation (not Active Directory / not domain-wide) |
| Requested by | Lab operator (portfolio project) |
| Status | Implemented and tested in lab |

## What Was Changed

Local Group Policy settings under **Account Lockout Policy**:

- Account lockout threshold: **5** invalid logon attempts
- Account lockout duration: **15** minutes
- Reset account lockout counter after: **15** minutes

A local test account (`LabTestUser`) was created to validate the change without locking the primary administrator session during testing.

## Why It Was Changed

- Reduce the risk of successful password guessing against local accounts.
- Practice documenting a security control change with clear validation evidence.
- Align lab work with access-control, authentication, logging, and change-management concepts referenced in ISO/IEC 27001:2022 Annex A (see final assessment).

## Expected Impact

| Area | Expected impact |
|---|---|
| Security | Failed logon attempts trigger temporary lockout after threshold is reached |
| Users | A user who mistypes a password repeatedly may be locked out for 15 minutes |
| Operations | Security Event Log should record failed logons (4625) and lockouts (4740) |
| Availability | Temporary account lockout may interrupt legitimate access until the duration expires or an admin unlocks the account |

## Validation Plan

1. Confirm policy values in Local Group Policy Editor (`gpedit.msc`).
2. Attempt repeated failed logons to `LabTestUser`.
3. Confirm the account lockout message on the sign-in screen.
4. Review Security logs for Event IDs **4625** and **4740**.

Validation results are recorded in [Test Plan and Results](../Testing/Test-Plan-and-Results.md).

## Rollback Plan

If the change causes unacceptable lockouts in this lab environment:

1. Open Local Group Policy Editor → Account Lockout Policy.
2. Set **Account lockout threshold** to `0` (disables lockout) or restore previous lab values.
3. Run `gpupdate /force` if needed.
4. Unlock any locked local account from an administrator session if required.
5. Re-test sign-in with the affected account.

## Approvals

No formal organizational approval was obtained. This change was performed only inside a personal lab virtual machine.
