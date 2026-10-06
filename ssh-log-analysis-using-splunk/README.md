# SSH Log Analysis using Splunk

**Local SOC Lab — SSH Authentication Analysis**

> Completed hands-on analysis of SSH logs using Splunk Enterprise.

## Quick Overview

This project documents a Splunk investigation of SSH authentication activity. I uploaded a JSON SSH log dataset, validated extracted fields, searched for failed logins, reviewed brute-force indicators, tracked successful logins, and identified connections that did not complete authentication.

| Field | Details |
| --- | --- |
| Role | SOC Analyst Learner |
| Environment | Local Splunk Enterprise lab |
| Scenario | SSH log analysis |
| Status | Completed |
| Data | JSON SSH log dataset |
| Index | `ssh_logs` |
| Source | `ssh_logs.json` |
| Events | 1,200 total SSH events |

## Investigation Process

```text
Upload SSH Logs → Validate JSON Fields → Count Event Types → Analyze Failed Logins
→ Review Brute-Force Indicators → Track Successful Logins
→ Identify Unauthenticated Connections → Document Findings
```

## Key Findings

- Splunk successfully ingested and parsed `ssh_logs.json`.
- The dataset contained 1,200 SSH-related events.
- Event categories included successful logins, failed logins, multiple failed authentication attempts, and connections without authentication.
- Failed SSH login analysis highlighted source IPs that generated repeated authentication failures.
- Multiple failed authentication attempts provided a stronger brute-force indicator.
- Connections without authentication may indicate SSH probing, scanning, or incomplete sessions.
- Successful login tracking can help identify suspicious access after repeated failures.

## View the Project

- **[Read the Full Walkthrough](full-walkthrough.md)** — workflow, SPL, findings, alert logic, and lessons learned
- **[View Evidence](screenshots/README.md)** — architecture image and named Splunk screenshots with captions
- **[View SPL Searches](searches/investigation.spl)** — searches used during the investigation

## Training Context

This project uses a provided lab dataset for educational purposes. The screenshots show my Splunk workflow and investigation results in a local lab environment. The repository includes documentation and screenshot evidence rather than the original dataset.

## Author

**Gabriel Akoi**

Cybersecurity Student | SOC Analyst Learner | Splunk & Security Monitoring
