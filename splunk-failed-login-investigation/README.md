# Splunk Failed Login Investigation

**Local SOC Lab — Authentication Analysis**

> Completed hands-on investigation using simulated authentication data.

## Quick Overview

This project documents an investigation of Windows-style authentication events in Splunk Enterprise. I reviewed login outcomes, targeted accounts and endpoints, source IP activity, rapid failures, and successful authentication following failed attempts.

| Field | Details |
| --- | --- |
| Role | SOC Analyst Learner |
| Environment | Local Splunk Enterprise lab |
| Scenario | Failed login investigation |
| Status | Completed |
| Data | Custom CSV authentication dataset |
| Index | `main` |
| Events | 15 total: 11 failed, 4 successful |

## Investigation Process

```text
Ingest Logs → Validate Events → Compare Login Outcomes → Review Users and IPs
→ Examine Endpoint Activity and Timing → Correlate Failed and Successful Logins
→ Document Findings
```

## Key Findings

- `admin` received six failed login attempts, the highest account total.
- `WIN-PC01` accounted for 10 of the 11 failed attempts.
- `203.0.113.50` generated three failures against `admin` within 29 seconds.
- `john` had two failed attempts followed by a successful login, all from `192.168.1.25`.

These patterns warrant further triage in a real environment; the supplied evidence does not establish account compromise.

## View the Project

- **[Read the Full Walkthrough](full-walkthrough.md)** — investigation questions, SPL, findings, and lessons learned
- **[View Evidence](screenshots/README.md)** — six named PNG screenshots with captions
- **[View SPL Searches](searches/investigation.spl)** — queries extracted from the supplied write-up
- **[View the SPL Cheat Sheet](resources/Splunk_Failed_Login_SPL_Cheat_Sheet.pdf)** — supplied reference PDF

## Training Context

This project uses simulated data for educational purposes. It documents learning in a local lab. The original CSV was not supplied, so the repository includes documentation and screenshots rather than a reconstructed dataset. Searches were not rerun as part of preparing this portfolio entry.

## Author

**Gabriel Akoi**

Cybersecurity Student | SOC Analyst Learner | Splunk & Security Monitoring
