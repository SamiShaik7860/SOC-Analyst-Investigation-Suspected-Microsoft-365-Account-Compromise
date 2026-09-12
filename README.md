# SOC Analyst Investigation: Suspected Microsoft 365 Account Compromise

> **Portfolio project:** Microsoft Sentinel / Microsoft Entra ID security investigation  
> **Role focus:** SOC Analyst | Security Analyst | Cyber Defense Analyst  
> **Data:** Simulated for training and portfolio use

## Project Overview

This project demonstrates how a SOC analyst can investigate a suspected identity compromise using Microsoft Sentinel, Microsoft Entra ID sign-in telemetry, KQL, and the MITRE ATT&CK framework.

The scenario begins with a high volume of failed authentication attempts against a user account, followed by a successful sign-in from an unusual location and suspicious PowerShell activity. The objective is to determine whether the activity represents a true positive, establish the scope of compromise, document evidence, assign severity, and recommend containment and remediation actions.

This repository focuses on **analyst work** rather than infrastructure deployment. It highlights:

- Alert triage
- Log analysis
- KQL hunting
- Identity investigation
- Timeline development
- MITRE ATT&CK mapping
- Incident severity assessment
- Root-cause analysis
- Containment recommendations
- Executive communication
- Incident documentation

---

## Scenario

At 02:14 UTC, Microsoft Sentinel generates an alert for repeated failed sign-in attempts against `alex.johnson@contoso.com`.

Approximately nine minutes later, the same account successfully authenticates from an IP address associated with a country not previously observed for the user. Shortly afterward, endpoint telemetry records an encoded PowerShell command.

The SOC analyst must determine:

1. Whether the successful authentication was legitimate.
2. Whether the failed attempts represent password spraying or targeted brute force.
3. Whether the PowerShell execution is related to the authentication activity.
4. Whether the account or endpoint should be contained.
5. What additional controls should be implemented.

---

## Environment / Tools

| Technology | Analyst Use |
|---|---|
| Microsoft Sentinel | SIEM alert triage, investigation, hunting |
| Microsoft Entra ID | Authentication and identity analysis |
| Microsoft Defender XDR | Endpoint and identity correlation |
| Kusto Query Language (KQL) | Threat hunting and log analysis |
| MITRE ATT&CK | Adversary behavior mapping |
| VirusTotal / WHOIS | Optional IP/domain enrichment |
| Excel / CSV | Timeline and evidence review |

---

## Investigation Workflow

### 1. Alert Triage

Initial alert:

- **Alert:** Multiple failed sign-ins followed by success
- **User:** `alex.johnson@contoso.com`
- **Source IP:** `185.220.101.42`
- **Initial Severity:** Medium
- **Primary concern:** Credential compromise

Questions asked during triage:

- Is the IP address known for this user?
- Did MFA succeed?
- How many authentication failures occurred?
- Were other users targeted by the same source?
- Was the successful authentication followed by suspicious activity?
- Is the device managed or compliant?

### 2. Authentication Analysis

The first query identifies repeated failed authentication attempts against the account.

See: [`kql/01_failed_logons.kql`](kql/01_failed_logons.kql)

Key observation:

- 27 failed sign-in attempts occurred within approximately 10 minutes.
- The attempts originated from the same public IP.
- The user historically signs in from the United States.
- The source IP was not previously observed for the account.

### 3. Successful Authentication After Failures

The second query looks for successful sign-ins occurring shortly after repeated failures.

See: [`kql/02_success_after_failures.kql`](kql/02_success_after_failures.kql)

Key observation:

- A successful authentication occurred nine minutes after the failed attempts began.
- The successful event originated from the same suspicious IP.
- The sign-in location was inconsistent with the user's normal activity.

### 4. Geographic Anomaly Review

The investigation then checks for unusual countries and possible impossible-travel behavior.

See:

- [`kql/03_impossible_travel.kql`](kql/03_impossible_travel.kql)
- [`kql/05_rare_country_signins.kql`](kql/05_rare_country_signins.kql)

Key observation:

- A U.S. sign-in occurred approximately 35 minutes before the foreign authentication.
- The locations are not realistically travelable within the observed timeframe.
- No approved VPN exit node was associated with the foreign IP.

### 5. Endpoint Activity Review

Endpoint activity was reviewed for suspicious process execution after the authentication event.

See: [`kql/04_suspicious_powershell.kql`](kql/04_suspicious_powershell.kql)

Key observation:

- PowerShell executed with an encoded command-line argument.
- The process occurred within 20 minutes of the suspicious sign-in.
- The behavior warranted escalation for possible post-compromise execution.

---

## Incident Timeline

| Time (UTC) | Event | Analyst Interpretation |
|---|---|---|
| 02:14 | Failed sign-ins begin | Possible brute-force activity |
| 02:18 | Failed attempts continue | Pattern exceeds normal user error |
| 02:23 | Successful sign-in from same IP | Potential credential compromise |
| 02:27 | New geographic location recorded | Identity anomaly |
| 02:41 | Encoded PowerShell observed | Possible post-compromise execution |
| 02:46 | SOC escalates incident | True-positive investigation |
| 02:52 | Account sessions revoked | Containment |
| 02:55 | Password reset initiated | Credential remediation |
| 03:02 | Endpoint isolated | Host containment |

---

## Findings

### Finding 1 — Repeated Authentication Failures

**Evidence:** 27 failed authentication attempts from the same IP in a short period.

**Assessment:** This activity is inconsistent with normal user behavior and is indicative of targeted credential guessing or brute-force activity.

### Finding 2 — Successful Login From Suspicious Source

**Evidence:** Successful authentication originated from the same IP responsible for the failed attempts.

**Assessment:** This materially increases the likelihood that valid credentials were obtained.

### Finding 3 — Geographic Anomaly

**Evidence:** The account authenticated from geographically distant locations within a timeframe inconsistent with physical travel.

**Assessment:** Possible session compromise, credential theft, VPN/proxy abuse, or token misuse.

### Finding 4 — Suspicious PowerShell Execution

**Evidence:** Encoded PowerShell was observed shortly after the suspicious authentication.

**Assessment:** Potential command execution following account compromise. Additional endpoint review is required to determine the payload and persistence mechanisms.

---

## Analyst Verdict

**Disposition:** True Positive  
**Final Severity:** High  
**Confidence:** High

### Rationale

The incident was escalated because multiple independent indicators aligned:

- Repeated failed authentication attempts
- Successful authentication from the same suspicious source
- Unusual geographic activity
- Temporal correlation with suspicious PowerShell execution
- No known business justification for the source IP or location

The combination of identity and endpoint evidence suggests a likely account compromise rather than a benign authentication anomaly.

---

## MITRE ATT&CK Mapping

| Technique | ID | Evidence |
|---|---|---|
| Brute Force | T1110 | Repeated authentication attempts |
| Valid Accounts | T1078 | Successful login using compromised credentials |
| PowerShell | T1059.001 | Encoded PowerShell execution |
| External Remote Services | T1133 | Suspicious remote authentication |
| Account Discovery* | T1087 | Potential follow-on investigation area |

\*Included as a potential follow-on behavior to hunt for; not confirmed in the simulated evidence.

Detailed mapping: [`docs/mitre-attack-mapping.md`](docs/mitre-attack-mapping.md)

---

## Containment and Remediation Recommendations

### Immediate Containment

1. Revoke active sessions and refresh tokens.
2. Reset the affected user's password.
3. Require MFA re-registration if compromise is suspected.
4. Isolate the affected endpoint.
5. Block confirmed malicious IP addresses where appropriate.
6. Review mailbox forwarding rules and OAuth consent grants.
7. Search for the same indicators across other users and endpoints.

### Short-Term Remediation

- Enforce phishing-resistant MFA for privileged and high-risk users.
- Review Conditional Access policies.
- Disable legacy authentication where still enabled.
- Enable risk-based identity protection policies.
- Tune Sentinel analytics for authentication bursts followed by success.
- Create watchlists for approved VPN and corporate egress IPs.

### Long-Term Improvements

- Implement stronger identity baselines.
- Review privileged access regularly.
- Use threat-intelligence enrichment for identity alerts.
- Build automated playbooks for session revocation and user containment.
- Conduct recurring identity-compromise tabletop exercises.

---

## Evidence Handling

For every major conclusion, the analyst should preserve:

- Query used
- Query time range
- Relevant timestamps
- User principal name
- IP address
- Device ID
- Correlation or incident ID
- Screenshot or exported result
- Analyst interpretation

Do not rely on screenshots alone. Preserve searchable evidence and document why each artifact matters.

---

## Repository Structure

```text
soc-analyst-sentinel-investigation/
├── README.md
├── docs/
│   ├── executive-summary.md
│   ├── incident-report.md
│   └── mitre-attack-mapping.md
├── kql/
│   ├── 01_failed_logons.kql
│   ├── 02_success_after_failures.kql
│   ├── 03_impossible_travel.kql
│   ├── 04_suspicious_powershell.kql
│   └── 05_rare_country_signins.kql
├── data/
│   └── sample_signins.csv
└── screenshots/
    └── README.md
```

---

## Skills Demonstrated

- Security alert triage
- Microsoft Sentinel investigation
- Microsoft Entra ID log analysis
- KQL threat hunting
- Incident correlation
- Authentication anomaly detection
- Endpoint investigation
- MITRE ATT&CK mapping
- Incident severity classification
- Root-cause analysis
- Security recommendations
- Technical and executive reporting

---

## How I Would Explain This Project in an Interview

> I built a simulated SOC investigation around a suspected Microsoft 365 account compromise. I started with repeated failed sign-ins, correlated them with a successful login from an unusual location, then pivoted into endpoint telemetry and identified suspicious encoded PowerShell activity. I used KQL to build the investigation timeline, mapped the behavior to MITRE ATT&CK, classified the incident as a high-severity true positive, and documented containment and remediation actions. The project is focused on the analyst workflow from alert triage through incident closure.

---

## Disclaimer

This project uses simulated identities, IP addresses, and event data for educational and portfolio purposes. No production credentials, customer information, or confidential organizational data is included.
