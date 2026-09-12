# SOC Analyst Playbook: Suspected Account Compromise

## Phase 1 — Validate the Alert

Review:

- Alert name
- Detection source
- User
- IP
- Timestamp
- Application
- Authentication result
- MFA result
- Device
- Location

Do not immediately classify the alert as malicious.

## Phase 2 — Establish User Baseline

Determine:

- Normal countries
- Normal IP ranges
- Normal devices
- Normal applications
- Typical sign-in times
- VPN usage
- Travel status

## Phase 3 — Review Authentication Pattern

Look for:

- Repeated failures
- Failure bursts
- Success after failures
- Multiple users from one source
- Multiple countries
- MFA failures
- Legacy authentication
- New devices

## Phase 4 — Scope the Source

Search the source IP across:

- Sign-in logs
- Endpoint logs
- Firewall logs
- Email security logs
- Threat intelligence
- Other identities

## Phase 5 — Review Post-Authentication Activity

Check for:

- PowerShell
- CMD
- Script interpreters
- Credential access
- Browser credential theft
- Mailbox forwarding
- OAuth grants
- New MFA registration
- Privilege changes
- Cloud resource access

## Phase 6 — Determine Disposition

Possible outcomes:

### False Positive

Evidence supports legitimate activity.

### Benign True Positive

The detection was technically correct, but activity was authorized.

### True Positive

Evidence supports malicious or unauthorized activity.

## Phase 7 — Determine Severity

Consider:

- Privilege level
- Sensitive systems
- Number of users
- Endpoint execution
- Persistence
- Data access
- Lateral movement
- Business impact

## Phase 8 — Containment

Potential actions:

- Revoke sessions
- Reset password
- Require MFA re-registration
- Disable identity
- Isolate device
- Block malicious infrastructure
- Remove malicious inbox rules
- Revoke OAuth grants

## Phase 9 — Document

Record:

- What happened
- How it was detected
- What evidence was reviewed
- What was confirmed
- What was not confirmed
- What actions were taken
- What improvements are recommended
