# Impact Assessment — Local Account Lockout Policy

Assessment for enabling local account lockout on a Windows 11 Pro ARM64 lab VM. This is a learning exercise, not a production risk assessment.

## Security Benefits

- Limits rapid repeated password guesses against local accounts.
- Makes brute-force attempts more visible through Security event logging.
- Encourages review of authentication failures as part of basic monitoring practice.
- Provides a concrete example of an access-control safeguard that can be tested and evidenced.

## Possible User Impact

| Scenario | Impact | Likelihood in this lab |
|---|---|---|
| Occasional mistyped password | Low — lockout only after 5 failures within the observation window | Low |
| Forgotten password + many retries | Account locked for 15 minutes or until unlocked by an administrator | Medium during testing |
| Shared local account misuse | Legitimate users may be locked out by someone else guessing the password | Low in single-user lab |
| Primary admin accidentally locked | Could delay recovery if no alternate admin path exists | Mitigated by using `LabTestUser` for testing |

## Risks and Mitigation

| Risk | Description | Mitigation used in this lab |
|---|---|---|
| Denial of service via lockout | An attacker who knows a username can force repeated failures | Used a dedicated test account; short 15-minute duration |
| Overly aggressive threshold | Too-low threshold increases help-desk / unlock friction | Threshold set to 5 |
| Incomplete logging | Lockout occurs but events are not reviewed | Reviewed Event IDs 4625 and 4740 after testing |
| False sense of security | Lockout alone does not equal strong authentication | Documented limitations; no MFA/domain claims made |
| Policy drift | Settings may be changed later without documentation | Settings and evidence captured in this repository |

## Business / Lab Continuity Notes

- Impact is limited to one virtual machine.
- No production users, domain controllers, or cloud tenants were affected.
- Rollback is straightforward via Local Group Policy Editor.

## Related ISO 27001 Themes

This change touches concepts related to access control, authentication protections, logging, monitoring, and change management. See [Security Controls Assessment](../Final-Report/Security-Controls-Assessment.md) for a careful Annex A mapping and explicit limitations.
