# Test Plan and Results

## Objective

Confirm that the local Windows account lockout policy locks `LabTestUser` after five failed sign-in attempts and that Windows Security logging records the expected events.

## Environment

| Item | Detail |
|---|---|
| Host | MacBook Pro M2 |
| Hypervisor | UTM |
| Guest OS | Windows 11 Pro ARM64 |
| Policy tool | Local Group Policy Editor (`gpedit.msc`) |
| Test account | `LabTestUser` (local account) |
| Scope | Local workstation only (not Active Directory) |

## Preconditions

- Account lockout threshold = 5
- Account lockout duration = 15 minutes
- Reset counter = 15 minutes
- Policy values verified in Local Group Policy Editor
- Test account exists and is not already locked

## Test Procedure

1. Sign out or switch user to the Windows sign-in screen.
2. Select `LabTestUser`.
3. Enter an incorrect password five times.
4. Observe the sign-in screen after the threshold is reached.
5. Open Event Viewer as an administrator:
   - Windows Logs → Security
6. Filter for Event IDs `4625` and `4740`.
7. Record the number of matching events and timestamps.

## Expected Results

| Check | Expected result |
|---|---|
| Lockout behavior | Account is locked after 5 failed attempts |
| User-visible message | Windows indicates the account is locked out |
| Failed logons | Five Event ID **4625** events |
| Lockout event | One Event ID **4740** event |
| Timing | Lockout event occurs at/near the fifth failure |

## Actual Results

| Check | Actual result | Outcome |
|---|---|---|
| Lockout behavior | `LabTestUser` was locked after repeated failed logons | **Pass** |
| User-visible message | “The referenced account is currently locked out and may not be logged on to.” | **Pass** |
| Failed logons | Five Event ID **4625** events between 7:18:21 AM and 7:18:36 AM on 11/19/2026 | **Pass** |
| Lockout event | One Event ID **4740** at 7:18:36 AM on 11/19/2026 for `LabTestUser` | **Pass** |

Overall result: **Pass**

## Event IDs Observed

| Event ID | Meaning | Count observed |
|---|---|---|
| 4625 | An account failed to log on | 5 |
| 4740 | A user account was locked out | 1 |

## Screenshot References

| Evidence | Path |
|---|---|
| Policy verification | [`../Evidence/Configuration/account-lockout-policy-verification.png`](../Evidence/Configuration/account-lockout-policy-verification.png) |
| Lockout screen | [`../Evidence/Testing/account-lockout-screen.png`](../Evidence/Testing/account-lockout-screen.png) |
| Failed logon events (4625) | [`../Evidence/Testing/failed-logon-events.png`](../Evidence/Testing/failed-logon-events.png) |
| Account lockout event (4740) | [`../Evidence/Testing/account-lockout-event-4740.png`](../Evidence/Testing/account-lockout-event-4740.png) |

## Notes

- An intermediate sign-in message (“Invalid credentials, delaying next attempt…”) was observed during failed attempts before the final lockout message. That delay message alone was not treated as lockout evidence.
- Event Viewer filtering for `4625,4740` was used to isolate relevant Security events.
- No production systems were tested.
