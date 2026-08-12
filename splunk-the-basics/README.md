# Splunk: The Basics

**TryHackMe — Splunk: The Basics**

> Completed SIEM Fundamentals Lab · Hands-On Splunk Training

## Quick Overview

This project documents a hands-on Splunk lab completed in a controlled TryHackMe environment. I imported newline-delimited JSON VPN logs into a dedicated index, validated the ingestion, extracted fields, and used Search Processing Language (SPL) to investigate users, source IP addresses, and countries.

> This was controlled cybersecurity training, not professional production SOC employment.

| Field | Details |
| --- | --- |
| Role | Simulated SOC Analyst |
| Environment | TryHackMe Splunk lab |
| Scenario | VPN log ingestion and analysis |
| Status | Completed |
| SIEM | Splunk Enterprise |
| Data | Newline-delimited JSON VPN logs |
| Index | `VPN_Logs` |

## Investigation Process

```text
Upload VPN Logs → Confirm Source Type → Configure Index and Host
→ Validate Event Count → Extract JSON Fields → Filter and Aggregate Events
→ Document Findings
```

## What This Project Demonstrates

- **SIEM Fundamentals:** Understanding the roles of indexers, search heads, and forwarders
- **Data Ingestion:** Uploading JSON logs and assigning them to a dedicated index
- **SPL Analysis:** Searching, filtering, extracting, and aggregating event data
- **VPN Investigation:** Correlating usernames, source IP addresses, and geographic fields
- **Validation:** Checking event counts and confirming that structured fields were parsed correctly

## View the Project

- **[Read the Full Walkthrough](full-walkthrough.md)** — architecture notes, ingestion steps, SPL searches, validation workflow, and lessons learned
- **[View Evidence](screenshots/)** — verified results and a catalog of the supplied lab screenshots

## Training Context

This case study records learning from a controlled TryHackMe simulation. It focuses on SIEM concepts, investigation methodology, and the searches used in the lab. Protected answers, credentials, flags, and fabricated results are intentionally excluded.
