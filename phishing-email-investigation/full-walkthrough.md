# Phishing Email Investigation

**TryHackMe SOC Simulator — Introduction to Phishing**

> Completed SOC Investigation · Hands-On SOC Training

A hands-on SOC Level 1 investigation completed within the TryHackMe SOC Simulator, focused on alert triage, phishing analysis, SIEM investigation, indicator validation, alert classification, documentation, and escalation workflows.

| Field | Details |
| --- | --- |
| Role | Simulated SOC Level 1 Analyst |
| Environment | TryHackMe SOC Simulator |
| Scenario | Introduction to Phishing |
| Status | Completed |
| SIEM | Splunk |

## 01 — Scenario Overview

The **Introduction to Phishing** scenario provided a simulated Security Operations Center environment where I worked in the role of a SOC Level 1 Analyst.

The environment provided access to an Alert Queue, SIEM platform, Live Analyst VM, investigation resources, and supporting documentation. I used these resources to review alerts, investigate suspicious activity, validate indicators, classify alerts, document findings, and determine whether escalation was required. I selected Splunk as the SIEM platform for the simulation.

## 02 — Why Splunk?

I selected Splunk because it is widely used for enterprise security monitoring and log analysis. I also wanted additional hands-on exposure to investigating security events through a widely adopted SIEM platform.

## 03 — My Role: SOC Level 1 Analyst

During the simulation, I worked as the first line of security alert triage. My responsibility was to review available information, assess severity, investigate suspicious activity, gather evidence, determine a classification, document findings, and identify whether escalation was necessary.

- Monitor incoming alerts
- Review alert severity
- Investigate suspicious email activity
- Determine email direction and context
- Review event and log information
- Analyze relevant indicators
- Validate indicators using investigation resources
- Classify investigated alerts
- Document investigation findings
- Determine whether escalation is required

## 04 — Alert Queue and Prioritization

The Alert Queue served as the starting point. Severity helped prioritize activity, with higher-severity alerts requiring more detailed investigation. A decision was not made from severity alone; additional evidence needed review before determining whether activity represented a genuine threat.

## 05 — Understanding Email Direction

### Inbound Email

**External Sender → Internal Recipient**

An email originating outside the organization and delivered internally.

### Outbound Email

**Internal Sender → External Recipient**

An email originating within the organization and sent externally.

Understanding direction provided additional context before reaching a classification decision.

## 06 — Investigation Workflow

### 1. Alert Received

Review the incoming security alert from the SOC Alert Queue.

### 2. Assess Severity

Review severity and available alert information to determine investigation priority.

### 3. Understand the Event

Review email and event context, including whether the activity is inbound or outbound.

### 4. Investigate with Splunk

Review relevant event and log information associated with the alert.

### 5. Identify Indicators

Identify relevant URLs, IP addresses, sender information, and other available observables.

### 6. Validate Indicators

Use VirusTotal and TryDetectThis to gather additional information about relevant indicators.

### 7. Correlate Evidence

Compare the Alert Queue, Splunk, reputation checks, investigation tools, and documentation.

### 8. Classify the Alert

Determine whether the evidence supports a True Positive or False Positive classification.

### 9. Document Findings

Prepare a clear SOC L1 report describing evidence, findings, and classification reasoning.

### 10. Escalate When Required

Document confirmed malicious activity for SOC Level 2 investigation and remediation when necessary.

## 07 — Investigation Tools

### Splunk

**Purpose:** SIEM / Log Investigation

Used to review security event and log information associated with alerts and gather context beyond the initial Alert Queue.

### VirusTotal

**Purpose:** Indicator Reputation Analysis

Used to review reputation information for relevant indicators. Results were one part of the investigation, not the sole basis for classification.

### TryDetectThis

**Purpose:** Additional Indicator Investigation

Used through the Live Analyst VM to support indicator validation and compare findings with other sources.

### TryHackMe SOC Simulator

**Purpose:** Simulated SOC Environment

Provided the Alert Queue, analyst workflow, Live Analyst VM, documentation, and controlled investigation environment.

## 08 — Correlating Evidence

An important part of the investigation was avoiding conclusions based on a single source. Information from the Alert Queue, Splunk, VirusTotal, TryDetectThis, and available documentation could be compared to build a clearer understanding of the activity.

```text
Alert Data + Logs + Indicators + Reputation + Context → Classification Decision
```

## 09 — Alert Classification

### True Positive

The alert correctly identified malicious or unauthorized activity. Evidence should be documented with enough context for escalation and response.

### False Positive

The alert triggered, but the investigation did not identify malicious activity that justified treating it as a confirmed incident. The decision still requires documented findings.

> These classifications explain the methodology practiced; they do not disclose or assign a classification to any specific protected scenario alert.

## 10 — SOC L1 Investigation Reporting

After investigating an alert, I documented the investigation so another analyst could understand what was reviewed, what was discovered, and why the decision was made.

### Reason for Classification

Explain why the available evidence supports the final alert classification.

### Investigation Findings

Document the relevant evidence gathered during the investigation.

### Attack Indicators

Document relevant indicators identified during the investigation.

### Escalation Decision

Explain whether additional investigation or response is required.

### Recommended Remediation

Document appropriate recommended actions when necessary.

## 11 — SOC Escalation

A SOC L1 Analyst performs initial triage and investigation. When malicious activity is confirmed and further response is required, findings should be documented clearly for escalation.

```text
Security Alert
      ↓
SOC L1 Triage
      ↓
Investigation
      ↓
Classification
      ↓
True Positive?
├── No  → Close / Document
└── Yes → Document → SOC L2 → Remediation
```

### Escalation Documentation

- What triggered the alert
- What was investigated
- What evidence was identified
- Which indicators were analyzed
- Why the alert was classified as malicious
- What further investigation or remediation may be required

## 12 — Skills Demonstrated

### SOC Operations

- SOC L1 Alert Triage
- Security Monitoring
- Alert Prioritization
- SOC Escalation

### Investigation

- Phishing Analysis
- Email Security Analysis
- SIEM Investigation
- Splunk Log Analysis
- Indicator Analysis
- IOC Analysis
- URL Reputation Analysis
- IP Reputation Analysis
- Evidence Correlation

### Documentation

- Alert Classification
- True Positive / False Positive Decision-Making
- Incident Documentation
- Investigation Reporting

## 13 — Key Lessons Learned

The simulation strengthened my understanding that a SOC L1 Analyst should not classify an alert from a single indicator or tool. A structured investigation requires alert context, relevant logs, indicator validation, evidence correlation, and clear reasoning.

### Investigate Before Classifying

An alert alone does not provide enough information to reach a reliable conclusion.

### Correlate Multiple Sources

Logs, indicators, reputation information, alert context, and supporting resources should be considered together.

### Document for the Next Analyst

Good documentation allows another SOC analyst to understand the evidence and continue the response efficiently.

## 14 — Training Context

This case study documents my hands-on learning within the **TryHackMe SOC Simulator — Introduction to Phishing** scenario. The work was performed in a controlled, simulated SOC environment for training and skill development. It is not presented as professional production SOC employment experience.

The case study focuses on investigation methodology, workflow, tools, reasoning, and lessons learned. Protected answers, flags, credentials, proprietary instructions, sensitive information, and fabricated evidence are intentionally excluded.

[← Back to project overview](README.md)
