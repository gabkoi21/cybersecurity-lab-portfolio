# Investigation Methodology

## 1. Alert Review

The investigation began in the SOC Alert Queue, where incoming alerts were reviewed and prioritized based on the information and severity provided.

## 2. Event Context

For phishing-related activity, I reviewed available email context, including whether the message was inbound or outbound.

- **Inbound:** External sender → Internal recipient
- **Outbound:** Internal sender → External recipient

## 3. Splunk Investigation

Splunk was used to review relevant event and log information associated with the alert and gather additional context beyond the initial Alert Queue data.

## 4. Indicator Identification

Relevant observables such as URLs, IP addresses, sender information, or other available indicators were identified for further review.

## 5. Indicator Validation

VirusTotal and TryDetectThis were used as additional investigation resources to validate relevant indicators.

These results were treated as supporting evidence, not as the sole basis for classification.

## 6. Evidence Correlation

Information from the Alert Queue, Splunk, VirusTotal, TryDetectThis, and available documentation was compared before reaching a decision.

## 7. Alert Classification

The alert was classified based on the combined investigation findings.

Possible classifications included:

- True Positive
- False Positive

## 8. Documentation

The SOC L1 report documented:

- Reason for classification
- Investigation findings
- Relevant indicators
- Escalation decision
- Recommended remediation

## 9. Escalation

When malicious activity required further response, the findings were documented clearly so they could be escalated to a SOC Level 2 Analyst.
