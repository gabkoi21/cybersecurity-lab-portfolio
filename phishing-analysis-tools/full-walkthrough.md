# Phishing Analysis Tools

**TryHackMe — Phishing Analysis**

> Completed Email Investigation Training · Hands-On SOC Skills

## 01 — Objective

This training built on email-header and email-body fundamentals by applying a repeatable workflow for phishing investigation. The objective was to collect useful artifacts, assess their reputation, and safely investigate suspicious attachments or URLs.

## 02 — Artifact Collection

I began by preserving the original message and collecting observables before making a decision. The core artifacts were:

- Sender and Reply-To addresses
- Recipient addresses, subject, and sent time
- Sending IP address, message-routing path, and mail-authentication results
- URLs in visible content and underlying HTML
- Attachment names, extensions, and SHA-256 hashes

This provides an auditable foundation for subsequent enrichment and correlation.

## 03 — Header Analysis

Full message headers were reviewed with header-analysis tools to quickly interpret routing and authentication details. Particular attention was given to the originating IP address, Reply-To inconsistencies, and SPF results. Authentication signals were treated as supporting evidence rather than a standalone phishing verdict.

## 04 — Body and Link Analysis

The message body was reviewed for urgency, impersonation, and calls to action. Embedded links were extracted from visible content and raw HTML, while shortened links were expanded for safe destination review. I used reputation and passive-analysis services instead of opening suspicious destinations directly.

## 05 — Attachment Handling

Attachments were handled only in a controlled environment. I used the following Linux command to create a stable file identifier for reputation lookups:

```bash
sha256sum suspicious_attachment
```

The resulting SHA-256 value can be queried in threat-intelligence services without uploading or executing the attachment on a production endpoint.

## 06 — Tools Applied

| Tool | Investigation purpose |
| --- | --- |
| Google Messageheader / Message Header Analyzer | Parse headers, routing, source IPs, and authentication signals |
| IPinfo | Identify IP ownership and geographic context |
| URLScan.io | Passively inspect website behavior and rendered content |
| Talos Reputation Center | Assess IP, domain, network, and hash reputation |
| VirusTotal | Correlate file, URL, IP, and domain detections across vendors |
| ANY.RUN / Hybrid Analysis / JOESandbox | Safely observe file and URL behavior in a sandbox |

## 07 — Sandbox Investigation

The attachment-analysis exercise demonstrated how a sandbox report can expose behavior that is not visible from the filename alone. I reviewed process activity, outbound network connections, contacted domains and IPs, downloaded content, and the report's exploit or threat classification. These observations were treated as IOCs to be corroborated with reputation data and email context.

## 08 — Decision Framework

```text
Header Context + Message Content + Link/Attachment Evidence
+ Reputation Results + Sandbox Behavior → Investigation Finding
```

A single reputation result or header value is not enough to prove intent. A defensible conclusion comes from correlating independent evidence and recording both the observed artifacts and their source.

## 09 — Skills Demonstrated

- Email header and body analysis
- SPF and email-authentication interpretation
- URL extraction and safe link investigation
- IP, domain, URL, and hash reputation analysis
- SHA-256 hashing on Linux
- Malware-sandbox report review
- IOC documentation and evidence correlation

## 10 — Key Lessons Learned

Phishing analysis is most effective when it is structured and cautious: preserve the source message, collect artifacts first, enrich them with multiple sources, and use isolated environments for potentially harmful files or URLs. Clear documentation ensures another analyst can reproduce the investigation and act on the identified indicators.

## Training Context

This write-up reflects training completed in a controlled TryHackMe environment. It intentionally omits protected answers, active malicious URLs and infrastructure, sample files, and fabricated evidence.

[← Back to project overview](README.md)
