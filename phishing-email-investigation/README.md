# Phishing Email Investigation

**TryHackMe SOC Simulator — Introduction to Phishing**

## Overview

This repository documents my hands-on learning experience from the **TryHackMe SOC Simulator — Introduction to Phishing** scenario.

I worked in a simulated **SOC Level 1 Analyst** role, where I reviewed incoming alerts, assessed severity, investigated phishing-related activity, analyzed indicators, reviewed relevant event data in Splunk, validated indicators using VirusTotal and TryDetectThis, classified alerts, documented findings, and practiced escalation workflows.

> **Training Context:** This work was completed in a controlled TryHackMe SOC simulation. It is not presented as professional production SOC employment experience.

## Environment

- **Platform:** TryHackMe SOC Simulator
- **Scenario:** Introduction to Phishing
- **Role:** Simulated SOC Level 1 Analyst
- **SIEM:** Splunk
- **Investigation Tools:** Splunk, VirusTotal, TryDetectThis, TryHackMe SOC Simulator

## Skills Demonstrated

- Alert Triage
- Phishing Analysis
- SIEM Investigation
- Splunk Log Analysis
- IOC Analysis
- Email Security Analysis
- Evidence Correlation
- Alert Classification
- Incident Documentation
- SOC Escalation

## Investigation Workflow

1. Review the alert in the Alert Queue
2. Assess alert severity
3. Understand the event and email direction
4. Investigate supporting event/log data in Splunk
5. Identify relevant indicators
6. Validate indicators with VirusTotal and TryDetectThis
7. Correlate evidence from multiple sources
8. Classify the alert
9. Document investigation findings
10. Escalate when additional response is required

## Repository Structure

```text
phishing-email-investigation/
├── README.md
├── docs/
│   ├── investigation-methodology.md
│   ├── findings-template.md
│   └── lessons-learned.md
├── screenshots/
│   └── README.md
├── .gitignore
└── LICENSE
```

## Public Write-Up Safety

This repository should not contain:

- TryHackMe flags
- Challenge answers
- Credentials
- Protected walkthrough solutions
- Sensitive information
- Unsanitized screenshots
- Fabricated evidence

Only sanitized, appropriate investigation documentation should be published.

## Portfolio

This investigation is designed to support the case study on my cybersecurity portfolio:

`/cybersecurity-projects/phishing-email-investigation`
