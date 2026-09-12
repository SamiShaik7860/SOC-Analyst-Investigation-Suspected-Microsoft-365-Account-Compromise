# Interview Guide

## 30-Second Explanation

> I built a simulated Microsoft Sentinel SOC investigation focused on account compromise. I analyzed repeated failed sign-ins, correlated them with a successful login from the same source, reviewed geographic anomalies, and pivoted into endpoint telemetry where I identified suspicious encoded PowerShell. I used KQL throughout the investigation, mapped the behavior to MITRE ATT&CK, scoped the environment, classified the incident as a high-severity true positive, and documented containment and remediation steps.

## 2-Minute Walkthrough

### 1. Detection

The investigation starts with repeated failed sign-ins against one user.

### 2. Triage

I determine whether the IP, device, location, and application are normal for the user.

### 3. Correlation

I correlate the failed attempts with a successful authentication from the same source.

### 4. Identity Analysis

I compare the login to the user's baseline and evaluate whether the location is plausible.

### 5. Endpoint Pivot

Because authentication succeeded, I review endpoint telemetry for suspicious execution.

### 6. Scoping

I search the suspicious IP across other accounts and devices.

### 7. ATT&CK Mapping

I map the confirmed behavior to relevant MITRE ATT&CK techniques.

### 8. Response

I recommend session revocation, password reset, MFA validation, and endpoint isolation.

## Likely Interview Questions

### Why is a successful login after failed attempts important?

It changes the incident from an unsuccessful attack attempt to a potential credential compromise.

### Why isn't impossible travel automatically malicious?

VPNs, cloud proxies, mobile networks, and legitimate travel can cause geographic anomalies.

### Is encoded PowerShell always malicious?

No. Administrators and software can use encoded PowerShell legitimately. Context determines risk.

### What makes the incident High severity?

The combination of successful suspicious authentication and possible endpoint execution indicates potential unauthorized access beyond a simple failed login alert.

### What would you check next?

- MFA details
- OAuth grants
- Mailbox rules
- Endpoint process tree
- Browser activity
- Additional users targeted
- Additional IPs
- Privilege escalation
- Data access
