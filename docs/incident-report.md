# Incident Report

## Incident Information

| Field | Value |
|---|---|
| Incident | Suspected Microsoft 365 Account Compromise |
| Category | Identity / Credential Compromise |
| Severity | High |
| Disposition | True Positive |
| Confidence | High |
| Status | Closed - Contained |
| Environment | Simulated |

## Affected Identity

`alex.johnson@contoso.com`

## Suspicious Source

`185.220.101.42`

## Affected Device

`FIN-WS-044`

## Detection

The investigation began after repeated failed authentication attempts were observed against the user account.

A successful authentication from the same source followed several minutes later.

## Evidence Summary

1. 27 failed authentication attempts from the same public IP.
2. Successful authentication from that same source.
3. Location inconsistent with the user's normal baseline.
4. Recent legitimate U.S. login made physical travel implausible.
5. Encoded PowerShell activity occurred shortly afterward.
6. No approved VPN or business reason explained the source.

## Analyst Assessment

The incident is more consistent with credential compromise than user error because:

- The failure count is unusually high.
- The source IP is unfamiliar.
- The same source later succeeds.
- The geographic behavior is abnormal.
- Endpoint activity increases the risk assessment.

## Root Cause

**Most likely root cause:** compromised credentials.

Possible credential theft mechanisms include:

- Credential phishing
- Password reuse
- Credential stuffing
- Malware-assisted theft

The exact initial credential theft mechanism is not confirmed by the available evidence.

## Scope

### Confirmed

- One cloud identity
- One suspicious public IP
- One endpoint under investigation

### Investigated

- Other accounts targeted by the same source
- Additional successful authentications
- Similar PowerShell activity
- Potential mailbox rule abuse
- OAuth application grants
- Additional endpoints

## Containment Actions

- Revoked active sessions.
- Reset the user's password.
- Required MFA validation.
- Isolated the endpoint.
- Searched for the suspicious IP across the environment.
- Reviewed mailbox rules.
- Reviewed OAuth grants.

## Recovery

Access should only be restored after:

- Password reset
- Session invalidation
- MFA confirmation
- Endpoint review
- Verification that no malicious sessions remain

## Lessons Learned

- Failed sign-ins should be correlated with later successes.
- Identity and endpoint telemetry should be investigated together.
- Geographic anomalies require VPN and proxy validation.
- Encoded PowerShell is suspicious only in context.
- Scoping is essential before incident closure.
