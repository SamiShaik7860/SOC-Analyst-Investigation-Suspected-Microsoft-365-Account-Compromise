# Executive Summary

## Incident Summary

A simulated security investigation identified evidence consistent with compromise of a Microsoft 365 user account.

The account experienced repeated authentication failures from an unfamiliar public IP address. The same source subsequently produced a successful authentication.

Additional analysis identified:

- Geographic activity inconsistent with the user's normal profile
- A recent U.S. sign-in that made legitimate physical travel unlikely
- Encoded PowerShell activity shortly after the suspicious authentication
- No known business justification for the suspicious source

## Business Risk

A compromised cloud identity can allow an attacker to:

- Access corporate email
- Access sensitive documents
- Impersonate an employee
- Create malicious mailbox rules
- Abuse OAuth applications
- Move laterally
- Establish persistence
- Target additional identities

## Final Assessment

**Severity:** High  
**Disposition:** True Positive  
**Confidence:** High

## Containment

The simulated response included:

- Session revocation
- Password reset
- MFA validation
- Endpoint isolation
- Indicator scoping
- Review of cloud identity activity

## Recommendation

The organization should prioritize phishing-resistant MFA, Conditional Access hardening, identity risk controls, improved authentication analytics, and stronger correlation between identity and endpoint telemetry.
