# Lab Setup Guide

## Goal

Reproduce the investigation workflow in a safe lab environment.

## Option 1 — Microsoft Sentinel

Use:

- Azure subscription
- Log Analytics workspace
- Microsoft Sentinel
- Microsoft Entra ID logs

Suggested workflow:

1. Create or use a lab Azure tenant.
2. Enable Microsoft Sentinel on a Log Analytics workspace.
3. Connect available identity data sources.
4. Open **Logs**.
5. Run the KQL queries in the `kql/` folder.
6. Capture screenshots of the results.
7. Replace simulated screenshots with your lab evidence.

## Option 2 — Microsoft Defender XDR

For endpoint-related queries:

1. Open Microsoft Defender.
2. Navigate to Advanced Hunting.
3. Run the endpoint query from:
   `kql/06_suspicious_powershell.kql`
4. Review process details and timeline context.

## Option 3 — Portfolio-Only Demonstration

If you do not have live telemetry:

1. Review the CSV files in `data/`.
2. Explain the investigation logic.
3. Show how each KQL query would be used.
4. Be explicit that the data is simulated.

## Recommended Screenshots

- Sentinel incident overview
- SigninLogs result set
- Authentication failure summary
- Success-after-failure correlation
- Rare country query
- PowerShell hunting result
- Defender device timeline
- Incident timeline

## Safety

Use only systems and tenants you are authorized to access.
