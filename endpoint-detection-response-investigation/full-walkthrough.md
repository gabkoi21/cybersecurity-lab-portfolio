# Endpoint Detection and Response Investigation

**TryHackMe — Introduction to EDR**

> Completed Endpoint Investigation · Hands-On SOC Training

A practical SOC investigation performed in a simulated Endpoint Detection and Response console, focused on understanding detections through endpoint telemetry and correlating processes, files, network activity, indicators, and threat-intelligence context.

| Field | Details |
| --- | --- |
| Role | Simulated SOC Analyst |
| Environment | TryHackMe Static EDR Dashboard |
| Scenario | Introduction to EDR |
| Status | Completed |
| Scope | Triage and visibility only |

## 01 — Scenario Overview

The scenario placed me in the role of a SOC analyst at the fictional organization **TECH THM**. The EDR console contained multiple medium- and high-severity detections requiring triage.

My task was to use the information available in each detection to reconstruct the activity on the affected endpoint. The exercise focused on understanding EDR visibility. Acknowledging alerts, isolating hosts, quarantining files, and performing other response actions were outside the task scope.

## 02 — What an EDR Provides

An Endpoint Detection and Response platform collects and correlates endpoint activity so analysts can investigate suspicious behavior. In this simulation, the console exposed:

- Detection summaries and severity
- Affected hosts and users
- Detection timestamps
- Process chains and parent-child relationships
- Executable paths, command lines, PIDs, hashes, and signatures
- File, registry, and network activity
- Indicators of compromise
- MITRE ATT&CK mappings and confidence scores
- Threat-intelligence labels
- Recorded response status and actions

## 03 — My Role in the Investigation

I approached each detection as an initial triage case. My responsibilities were to:

- Identify the affected host and user
- Review the detection severity and behavioral summary
- Reconstruct the process chain
- Inspect executable paths and command lines
- Determine whether files were downloaded, created, or executed
- Review network destinations and data-transfer activity
- Correlate IOCs with the observed endpoint behavior
- Use threat-intelligence context to reduce false assumptions
- Record concise, evidence-based findings

## 04 — Investigation Workflow

### 1. Review the Detection

I began with the summary, severity, affected host, user, timestamp, and confidence score to understand the alert context.

### 2. Analyze the Process Chain

I followed the parent-child process sequence to identify which application initiated the activity and which processes it spawned.

### 3. Inspect Process Details

I reviewed executable paths, command lines, PIDs, digital-signature information, hashes, user context, and process behavior.

### 4. Review Endpoint Activity

I checked file, registry, and network telemetry to identify downloads, dropped files, persistence attempts, outbound connections, or possible data transfer.

### 5. Correlate Indicators

I compared file paths, filenames, domains, IP addresses, URLs, and hashes against the detection narrative and threat-intelligence context.

### 6. Document the Finding

I recorded what happened, which evidence supported the conclusion, and whether the observed behavior appeared suspicious or expected in the lab context.

## 05 — Detection Case: Malware Staging on DESKTOP-HR01

### Detection Summary

A macro-enabled Microsoft Word document, `invoice.docm`, was opened by `WINWORD.EXE`. Word spawned `CMD.EXE`, which then launched `cURL.EXE` to download an executable from an external domain. The payload was saved in a public directory but was not executed.

This sequence was consistent with malware staging: an initial document triggered a command shell, a native transfer utility retrieved a payload, and the file was placed on disk for possible later execution.

| Detection Field | Observed Value |
| --- | --- |
| Host | `DESKTOP-HR01` |
| User | `alice.thomas` |
| Severity | High |
| Detection Time | Aug 9, 2026 at 22:25 |
| MITRE ATT&CK Tactic | Initial Access |
| Technique | T1566.001 — Spearphishing Attachment |
| Confidence Score | 95 |

### Process Chain

```text
explorer.exe
└── WINWORD.EXE /n "invoice.docm"
    └── CMD.EXE
        └── cURL.EXE
            └── install.exe (written to disk; not executed)
```

The important relationship was not merely that `cURL.EXE` existed on the endpoint. The suspicious context came from Microsoft Word spawning a command shell, which launched cURL immediately after a macro-enabled document was opened.

### Key Evidence

| Evidence Type | Value | Significance |
| --- | --- | --- |
| Download utility | `cURL.exe` | Launched by `CMD.exe` to retrieve the payload |
| Downloaded file | `C:\Users\Public\install.exe` | Payload written to a commonly accessible location |
| Source document | `C:\User\alice\Downloads\invoice.docm` | Macro-enabled document associated with the process chain |
| Domain | `ayebd.thm` | Identified by the simulation as the external server |
| IP address | `1.161.138.92` | External download infrastructure recorded by EDR |
| Payload SHA-256 | `9e107d9d372bb6826bd81d3542a419d6eaf1e3f5b94fc3b1f69413c5c30ef2e5` | Unknown executable analyzed and flagged in the lab |

### Analyst Finding

The telemetry showed a suspicious Office-to-shell-to-download chain. Although the downloaded file did not execute, its presence on disk represented a staged payload and justified the high-severity detection. The absence of execution limited the observed impact but did not make the behavior benign.

## 06 — Detection Case: Suspicious Activity on WIN-ENG-LAPTOP03

The investigation of `WIN-ENG-LAPTOP03` required correlating an executable in a temporary user directory with outbound network activity.

### Key Evidence

| Evidence Type | Observed Value | Significance |
| --- | --- | --- |
| Suspicious executable | `C:\Users\haris.khan\AppData\Local\Temp\syncsvc.exe` | Executable located in a user-writable temporary directory |
| Network destination | `https://files-wetransfer.com/upload/session/ab12cd34ef56/dump_2025.dmp` | Destination associated with the simulated exfiltration attempt |

### Analyst Finding

The executable name alone was insufficient to determine intent. Its location in a temporary directory and its association with an upload path ending in a memory-dump filename provided stronger behavioral context. Together, those artifacts supported investigation of a possible data-exfiltration attempt.

## 07 — Detection Case: UpdateAgent.exe on DESKTOP-DEV01

The `DESKTOP-DEV01` detection demonstrated why threat-intelligence and organizational context matter during triage.

Threat Intelligence labeled `UpdateAgent.exe` as a **known internal IT utility tool**. This context helped distinguish an approved organizational utility from malware that might use a similarly generic filename.

### Analyst Finding

File names are not reliable verdicts. An analyst should correlate the path, signer, hash, behavior, parent process, host context, and trusted internal intelligence before deciding whether an executable is malicious or expected.

## 08 — Process-Tree Analysis

Process trees allow an analyst to reconstruct execution lineage. Each process can look ordinary in isolation, while the complete chain reveals suspicious behavior.

```text
Document opened
      ↓
Office application starts
      ↓
Command shell spawned
      ↓
Native download utility launched
      ↓
Executable written to disk
```

In the `DESKTOP-HR01` case, `WINWORD.EXE`, `CMD.EXE`, and `cURL.EXE` are legitimate programs. Their sequence and command context made the activity suspicious.

## 09 — IOC Correlation

The IOC section connected endpoint events to relevant observables:

- **File paths** showed where suspicious artifacts were stored
- **File names** helped connect process and filesystem activity
- **Domains and IP addresses** identified external infrastructure
- **URLs** exposed the specific network resource involved
- **Hashes** provided a stable identifier for file comparison and intelligence checks

No single IOC was treated as sufficient proof. The strongest conclusion came from matching indicators with the process chain, timestamps, host, user, and behavior summary.

## 10 — MITRE ATT&CK Context

The console mapped the malicious-document detection to **Initial Access** and **T1566.001 — Spearphishing Attachment**. This mapping helped describe how the activity began, while the underlying telemetry explained what occurred on the endpoint.

MITRE ATT&CK labels support investigation and reporting, but they do not replace evidence review. The process lineage, command execution, file creation, and network activity remained the primary evidence.

## 11 — Detection and Response Visibility

The EDR displayed action-related fields such as blocked domains, quarantined files, flagged executables, and detection acknowledgement status. These fields helped show what the platform had observed or recorded.

Because response actions were outside the exercise scope, I did not treat the simulation as a live containment activity. In a real environment, confirmed malicious activity could require host isolation, file quarantine, credential review, broader IOC searches, and escalation according to the organization’s incident-response procedures.

## 12 — Skills Demonstrated

### SOC Operations

- Medium- and high-severity alert triage
- Endpoint detection review
- Host and user scoping
- Evidence-based investigation documentation
- Recognition of escalation and response boundaries

### Endpoint Investigation

- Parent-child process analysis
- Command-line interpretation
- Executable path analysis
- File and network activity review
- Detection timeline reconstruction
- Legitimate-tool abuse recognition

### Threat Analysis

- IOC correlation
- Suspicious download analysis
- Possible exfiltration identification
- Threat-intelligence interpretation
- MITRE ATT&CK mapping
- Differentiation of suspicious behavior from known internal utilities

## 13 — Key Lessons Learned

### Context Makes a Process Suspicious

Legitimate binaries such as Word, CMD, and cURL can participate in malicious activity. Their parent-child relationships, commands, timing, and resulting files establish the meaningful context.

### A Saved Payload Still Matters

A payload does not need to execute to represent risk. Writing a suspicious executable to disk may indicate staging and provides an artifact for containment and further analysis.

### Paths Reveal Intent and Risk

Executables running from public or user-writable temporary directories deserve additional scrutiny, especially when paired with unusual network activity.

### Threat Intelligence Must Be Correlated

Threat-intelligence labels can explain expected internal tools, but they should be considered alongside signatures, hashes, paths, behaviors, and organizational knowledge.

### EDR Triage Is Evidence Reconstruction

The most useful view came from connecting the alert summary, process chain, endpoint artifacts, network activity, IOCs, and threat intelligence into one coherent timeline.

## 14 — Training Context

This case study documents hands-on learning in the **TryHackMe Introduction to EDR** scenario. The work was completed in a controlled, simulated environment for training and portfolio development. It is not presented as professional production SOC experience.

The case study focuses on investigation methodology and selected scenario evidence. Credentials, flags, proprietary instructions, and fabricated evidence are intentionally excluded.

[← Back to project overview](README.md)
