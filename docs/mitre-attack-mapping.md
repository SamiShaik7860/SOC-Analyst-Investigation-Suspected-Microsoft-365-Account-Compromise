# MITRE ATT&CK Mapping

## T1110 — Brute Force

### Evidence

Repeated authentication attempts occurred against the same identity from a single unfamiliar source.

### Analyst Interpretation

The behavior is consistent with credential guessing.

---

## T1078 — Valid Accounts

### Evidence

The suspicious source successfully authenticated after repeated failures.

### Analyst Interpretation

Valid credentials may have been obtained or guessed.

---

## T1059.001 — PowerShell

### Evidence

Encoded PowerShell execution occurred after the suspicious authentication.

### Analyst Interpretation

PowerShell is a legitimate administration tool, but the timing and context increase concern.

---

## T1133 — External Remote Services

### Evidence

The identity was accessed remotely from an unfamiliar external source.

### Analyst Interpretation

External access channels may be abused after credential compromise.

---

## T1087 — Account Discovery

### Evidence

Not confirmed in this simulation.

### Analyst Interpretation

Account discovery is included as a follow-on hunting hypothesis, not a confirmed technique.

---

## Mapping Principle

MITRE ATT&CK should describe observed or strongly supported behavior.

Do not map techniques simply to make an incident appear more advanced.
