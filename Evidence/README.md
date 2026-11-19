# Evidence Index

Screenshots collected during the Windows 11 local account lockout lab (11/19/2026). Images were copied from the original captures and renamed to stable evidence filenames. Originals were preserved outside this repository’s required evidence set.

## Configuration

| File | Status | Description |
|---|---|---|
| [`Configuration/account-lockout-policy-verification.png`](Configuration/account-lockout-policy-verification.png) | Present | Local Group Policy Editor showing account lockout duration **15 minutes**, threshold **5 invalid logon attempts**, and reset counter **15 minutes**. |

## Testing

| File | Status | Description |
|---|---|---|
| [`Testing/account-lockout-screen.png`](Testing/account-lockout-screen.png) | Present | Windows sign-in screen for `LabTestUser` stating the account is currently locked out. |
| [`Testing/failed-logon-events.png`](Testing/failed-logon-events.png) | Present | Event Viewer Security log filtered to Event IDs 4625 and 4740, showing five failed logon (4625) events. |
| [`Testing/account-lockout-event-4740.png`](Testing/account-lockout-event-4740.png) | Present | Event Properties for Event ID **4740** confirming lockout of `LabTestUser`. |

## Missing Evidence

None of the four required evidence files are missing.

## Privacy Notes

Before sharing publicly, review visible lab identifiers:

- Local test account name: `LabTestUser`
- Guest computer name shown in Event 4740 details
- “Activate Windows” watermark from the unactivated lab VM

No passwords are included in these screenshots.
