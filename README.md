# SOC Detection Lab

Hands-on SOC lab for detecting and correlating multi-stage attacks using
Windows Event Logs, Sysmon and Splunk.

## Overview

A practical SOC environment used to simulate real-world attacks and build
detection, investigation and response skills: log analysis, event
correlation, SIEM dashboards and MITRE ATT&CK mapping.

## Lab environment

- Windows (victim, with Sysmon)
- Kali Linux (attacker)
- Sysmon (endpoint telemetry)
- Splunk (SIEM)
- VirtualBox

## Repository structure

```
soc-detection-lab/
├── brute-force/      # RDP brute force detection (4625 → 4624)
├── powershell/       # Suspicious PowerShell (Script Block Logging 4104)
├── reverse-shell/    # Reverse shell detection (Sysmon 1 + 3)
└── splunk/
    ├── dashboards/   # Dashboard XML + screenshot
    └── queries/      # SPL detection queries
```

## Attack scenarios

| # | Scenario | Key events | MITRE ATT&CK | Status |
|---|----------|-----------|--------------|--------|
| 1 | Brute force (RDP) | 4625, 4624 | T1110, T1078 | Done |
| 2 | Suspicious PowerShell | 4104, Sysmon 1 | T1059.001 | Done |
| 3 | Reverse shell | Sysmon 1, Sysmon 3, 4104 | T1059.001, T1071 | Done |
| 4 | Masquerading | Sysmon 1 | T1036 | In progress |

Each scenario has its own folder with attack steps, log analysis, detection
logic, investigation findings and screenshots.

## Splunk detection dashboard

A Splunk dashboard built on Windows authentication logs:

- Failed logins aggregated by source IP (4625)
- Source IP ↔ targeted account correlation
- Time-based attack-spike charts
- Severity scoring (LOW / MEDIUM / HIGH)
- MITRE ATT&CK mapping (T1110, T1078)

See `splunk/dashboards/dashboard.png` and `splunk/queries/` for details.

## Example detection query (brute force)

```spl
index=windows EventCode=4625
| eval src=coalesce(Source_Network_Address, IpAddress, WorkstationName)
| eval user=coalesce(TargetUserName, Account_Name)
| search NOT src IN ("-","::1")
| stats count as attempts by src, user
| eval severity=case(attempts>=10,"HIGH", attempts>=5,"MEDIUM", true(),"LOW")
| sort -attempts
```

## Skills demonstrated

Log analysis · event correlation · endpoint telemetry (Sysmon) · SIEM
(Splunk) · detection engineering fundamentals · alert triage · MITRE
ATT&CK mapping

## Roadmap

- Finish scenario 4 (masquerading / persistence detection)
- Add persistence detections (Scheduled Tasks, Run keys, Sysmon 13)
- Multi-stage correlation (PowerShell → reverse shell → persistence)
- Expand the environment (more endpoints, Wazuh integration)

## Status

In progress — continuously adding detection scenarios and refining analysis.
