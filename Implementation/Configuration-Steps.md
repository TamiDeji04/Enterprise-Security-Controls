# Configuration Steps

These steps reproduce the local account lockout configuration used in this lab on **Windows 11 Pro ARM64** running in **UTM** on a MacBook Pro M2.

Administrative access to the Windows guest is required.

## 1. Open Local Group Policy Editor

1. Sign in to Windows with a local administrator account.
2. Press `Win + R`, type `gpedit.msc`, and press Enter.
3. Navigate to:

```text
Computer Configuration
  → Windows Settings
    → Security Settings
      → Account Policies
        → Account Lockout Policy
```

## 2. Configure Account Lockout Settings

Set the following values:

| Policy | Value |
|---|---|
| Account lockout threshold | 5 invalid logon attempts |
| Account lockout duration | 15 minutes |
| Reset account lockout counter after | 15 minutes |

Notes:

- When you set the threshold, Windows may prompt you to configure the related duration and reset values.
- Confirm each setting in the policy list before closing the editor.

## 3. Create a Test Account (Optional but Recommended)

Use a dedicated local account so testing does not lock out your primary admin session.

1. Open **Settings → Accounts → Other users**.
2. Add a local account (for this lab: `LabTestUser`).
3. Set a known password for controlled testing.
4. Keep the account as a standard local user unless testing requires otherwise.

Do not store real passwords in this repository.

## 4. Verify the Configuration

### Method A — Local Group Policy Editor

Re-open `gpedit.msc` and confirm the Account Lockout Policy values match the table above.

Evidence: [`Evidence/Configuration/account-lockout-policy-verification.png`](../Evidence/Configuration/account-lockout-policy-verification.png)

### Method B — Command line (optional)

In an elevated Command Prompt or PowerShell, run:

```text
net accounts
```

Confirm:

- Lockout threshold: `5`
- Lockout duration (minutes): `15`
- Lockout observation window (minutes): `15`

## 5. Apply / Refresh Policy (If Needed)

Local security policy changes usually apply immediately. If values do not appear updated:

```text
gpupdate /force
```

Then re-check with Method A or Method B.

## 6. Proceed to Testing

Follow [Test Plan and Results](../Testing/Test-Plan-and-Results.md) to validate lockout behavior and review Security event logs.
