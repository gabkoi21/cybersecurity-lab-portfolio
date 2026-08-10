# Endpoint Detection and Response Investigation

**TryHackMe — Introduction to EDR**

> Completed Endpoint Investigation · Hands-On SOC Training

## Quick Overview

This project documents a hands-on endpoint investigation completed in a simulated TryHackMe EDR environment. I triaged medium- and high-severity detections by reviewing alert summaries, process chains, command-line activity, file paths, network connections, indicators of compromise, threat-intelligence labels, and recorded response actions.

> This was controlled cybersecurity training, not professional production SOC employment.

| Field | Details |
| --- | --- |
| Role | Simulated SOC Analyst |
| Environment | TryHackMe Static EDR Dashboard |
| Scenario | Introduction to EDR |
| Status | Completed |
| Security Platform | Endpoint Detection and Response console |
| Investigation Scope | Detection visibility and triage; acknowledgement and response actions were out of scope |

## Investigation Process

```text
Review Detection → Confirm Host and User → Analyze Process Chain
→ Inspect Files and Commands → Review Network Activity and IOCs
→ Correlate Threat Intelligence → Document Findings
```

## What This Project Demonstrates

- **Alert Triage:** Review of severity, timestamps, affected endpoints, users, and detection context
- **Endpoint Analysis:** Parent-child process analysis, command-line review, executable path validation, and file activity investigation
- **Threat Analysis:** IOC correlation, network destination review, MITRE ATT&CK mapping, and threat-intelligence interpretation
- **Documentation:** Evidence-based findings that distinguish suspicious behavior from known internal activity

## View the Project

- **[Read the Full Walkthrough](full-walkthrough.md)** — the complete case study covering the scenario, EDR visibility, investigation workflow, process analysis, evidence, findings, and lessons learned
- **[View Screenshots](screenshots/)** — guidance for adding sanitized visual evidence suitable for public sharing

## Training Context

This case study records learning from a controlled TryHackMe simulation. It presents the investigation method and a limited set of supplied lab artifacts for portfolio demonstration. No credentials, flags, proprietary room instructions, or fabricated evidence are included.
