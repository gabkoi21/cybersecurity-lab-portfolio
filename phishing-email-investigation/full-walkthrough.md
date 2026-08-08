# Full Walkthrough — Phishing Email Investigation

## Scenario and Role

The TryHackMe **Introduction to Phishing** scenario provided a controlled SOC environment with an Alert Queue, SIEM platform, Live Analyst VM, investigation resources, and supporting documentation. I worked as a simulated SOC Level 1 Analyst—the first line of alert triage.

My responsibilities included monitoring and prioritizing alerts, investigating suspicious email activity, reviewing logs, identifying and validating indicators, correlating evidence, classifying alerts, documenting findings, and deciding whether escalation was required.

I selected Splunk because it is widely used for enterprise security monitoring and log analysis, and I wanted more hands-on practice investigating security events with a widely adopted SIEM. This was training experience, not professional Splunk administration or employment.

## Investigation Workflow

### 1. Review the Alert

I began in the Alert Queue, reviewed the incoming alert, assessed its severity, and determined its investigation priority.

### 2. Understand the Email Event

I reviewed the available sender, recipient, and event context and determined the direction of the email:

- **Inbound:** External sender → Internal recipient
- **Outbound:** Internal sender → External recipient

Email direction helped establish the context and potential exposure.

### 3. Investigate in Splunk

I used Splunk to review relevant security events and log information associated with the alert. This provided additional context beyond the initial Alert Queue data.

### 4. Identify Indicators

I reviewed the available data for observables such as URLs, IP addresses, sender information, domains, and other indicators relevant to the alert.

### 5. Validate Indicators

I used two supporting resources:

- **VirusTotal:** Reviewed available reputation information for indicators such as URLs and IP addresses.
- **TryDetectThis:** Provided additional indicator investigation through the TryHackMe Live Analyst VM.

Reputation results were supporting evidence, not the sole basis for a classification.

### 6. Correlate the Evidence

I compared the alert information, Splunk logs, identified indicators, reputation results, supporting documentation, and overall event context.

```text
Alert Data + Logs + Indicators + Reputation + Context
                          ↓
              Classification Decision
```

### 7. Classify the Alert

- **True Positive:** The evidence supports that the alert correctly identified malicious or unauthorized activity.
- **False Positive:** The alert triggered, but the investigation did not identify malicious activity requiring treatment as a confirmed security incident.

The final decision depended on correlated evidence rather than one tool or indicator.

### 8. Document and Escalate

A complete SOC L1 report should record:

- Reason for classification
- Investigation findings
- Relevant attack indicators
- Escalation decision
- Recommended remediation, when appropriate

If the evidence supports malicious activity requiring further response, the SOC L1 Analyst documents the completed work and escalates it to SOC L2. Clear documentation enables the next analyst to continue without unnecessarily repeating the investigation.

```text
Security Alert → SOC L1 Triage → Investigation → Classification
                                              ↓
                         False Positive ─ Close and document
                         True Positive  ─ Document and escalate to SOC L2
```

## Lessons Learned

### Investigate Before Classifying

An alert alone does not contain enough information for a reliable classification. It is the starting point for the investigation, not the conclusion.

### Correlate Multiple Sources

Alert context, logs, indicators, reputation information, and investigation resources create a stronger basis for a decision when considered together.

### Document for the Next Analyst

Good investigation notes show what was reviewed, what was found, and why the evidence supports the decision. This makes escalation and continued response more efficient.

## Skills Practiced

- Alert triage
- Phishing analysis
- SIEM and Splunk log analysis
- IOC analysis
- Email security analysis
- Evidence correlation
- Alert classification
- Incident documentation
- SOC escalation

## Public-Safety Note

This walkthrough intentionally excludes TryHackMe flags, challenge answers, credentials, passwords, API keys, protected walkthrough solutions, personally identifiable information, sensitive organizational information, unsanitized screenshots, and fabricated investigation evidence. Actual alert-specific findings should be added only after they have been reviewed and sanitized for public sharing.
