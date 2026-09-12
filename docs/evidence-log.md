# Evidence Log

| Evidence ID | Timestamp UTC | Source | Artifact | Analyst Interpretation |
|---|---|---|---|---|
| EV-001 | 2026-09-10 01:48 | Entra ID | Normal U.S. sign-in | Establishes user baseline immediately before incident |
| EV-002 | 2026-09-10 02:14 | Entra ID | Failed login from suspicious IP | Beginning of credential attack pattern |
| EV-003 | 2026-09-10 02:18 | Entra ID | Continued failures | Automated behavior increasingly likely |
| EV-004 | 2026-09-10 02:23 | Entra ID | Successful login from same IP | Strong compromise indicator |
| EV-005 | 2026-09-10 02:27 | Entra ID | German location | Geographic anomaly |
| EV-006 | 2026-09-10 02:41 | Defender | Encoded PowerShell | Possible post-compromise execution |
| EV-007 | 2026-09-10 02:52 | Response | Sessions revoked | Identity containment |
| EV-008 | 2026-09-10 03:02 | Response | Endpoint isolated | Host containment |

## Evidence Handling Notes

For real investigations, preserve:

- Query text
- Time range
- Incident ID
- User principal name
- IP address
- Device ID
- Process command line
- Hashes
- Screenshots
- Exported results
- Analyst notes

Never commit production evidence to a public portfolio repository.
