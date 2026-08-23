# Phishing Analysis Tools

**TryHackMe — Phishing Analysis**

> Completed Email Investigation Training · Hands-On SOC Skills

## Quick Overview

This project documents hands-on phishing-analysis training focused on collecting email artifacts, examining headers and message bodies, validating IPs and URLs, and safely investigating suspicious attachments in sandbox environments.

> This work was completed in a controlled training environment. It does not include live malicious links, protected answers, or executable samples.

| Field | Details |
| --- | --- |
| Role | Security Analyst (training) |
| Environment | TryHackMe |
| Module | Phishing Analysis |
| Status | Completed |
| Tools | Messageheader, IPinfo, URLScan.io, Talos, VirusTotal, ANY.RUN, Hybrid Analysis, JOESandbox |

## Investigation Process

```text
Preserve Email → Extract Header Artifacts → Review Body and Attachments
→ Validate IPs, URLs, and Hashes → Safely Detonate When Required
→ Correlate Evidence → Document Findings
```

## What This Project Demonstrates

- **Email analysis:** Sender, recipient, Reply-To, subject, timestamps, routing, and authentication-result review
- **IOC collection:** URL extraction, shortened-link expansion, attachment-name review, and SHA-256 hashing
- **Threat intelligence:** IP, domain, URL, and file-reputation lookups across multiple sources
- **Safe analysis:** Use of controlled sandbox environments to observe file and URL behavior without direct interaction
- **Reporting:** Evidence-based documentation of notable processes, network activity, IOCs, and suspected exploitation behavior

## View the Project

- **[Read the Full Walkthrough](full-walkthrough.md)** — concise methodology, tools, and lessons learned

## Training Context

This case study covers security-analysis methods learned in a controlled lab. It intentionally excludes room answers, flags, active malicious infrastructure, credentials, and unverified claims.
