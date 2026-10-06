# SSH Log Analysis using Splunk

## Project Overview

This project demonstrates how Splunk can be used as a SIEM tool to analyze SSH authentication logs. The investigation focused on failed logins, successful logins, brute-force indicators, and SSH connections that did not complete authentication.

The lab was completed in a local Splunk Enterprise environment using a provided JSON dataset named `ssh_logs.json`.

---

## Objective

The main objectives of this project were to:

* Ingest JSON-formatted SSH logs into Splunk.
* Validate that important SSH fields were extracted correctly.
* Count SSH activity by event type.
* Identify source IPs generating failed login attempts.
* Detect multiple failed authentication attempts that may indicate brute force activity.
* Track successful SSH logins by source and destination.
* Identify unauthenticated SSH connections that may indicate probing or scanning.
* Practice creating visualizations and alert logic from SPL searches.

---

## Lab Environment

**SIEM:** Splunk Enterprise  
**Environment:** Local SOC lab  
**Data Source:** JSON SSH logs  
**Index:** `ssh_logs`  
**Source:** `ssh_logs.json`  
**Total Events:** 1,200

### Dataset Fields

The dataset included the following fields:

* `event_type`
* `auth_success`
* `auth_attempts`
* `id.orig_h`
* `id.orig_p`
* `id.resp_h`
* `id.resp_p`
* `proto`
* `ts`
* `uid`

The main event categories were:

* `Successful SSH Login`
* `Failed SSH Login`
* `Multiple Failed Authentication Attempts`
* `Connection Without Authentication`

---

# Investigation Workflow

I followed a question-driven investigation approach:

**Question → Evidence Needed → Fields → SPL → Result → Interpretation**

This kept the investigation focused on security questions instead of only running random searches.

---

## 1. How Were The SSH Logs Ingested?

I used Splunk's **Add Data** workflow and selected the upload option to ingest the local SSH log file.

The uploaded file was:

`ssh_logs.json`

The logs were stored in a dedicated index:

`ssh_logs`

This made the investigation easier to scope and kept the lab data separate from other Splunk data.

---

## 2. Were JSON Fields Extracted Correctly?

After upload, Splunk displayed a preview of the parsed JSON events.

The preview showed useful SSH fields such as:

* `event_type`
* `auth_success`
* `auth_attempts`
* `id.orig_h`
* `id.resp_h`
* `id.resp_p`
* `ts`
* `uid`

This confirmed that the logs were usable for field-based SPL searches.

---

## 3. What Types Of SSH Events Were Present?

### SPL

```spl
index=ssh_logs source="ssh_logs.json"
| stats count by event_type
```

### Finding

| Event Type | Count |
| --- | ---: |
| Successful SSH Login | 306 |
| Failed SSH Login | 305 |
| Multiple Failed Authentication Attempts | 303 |
| Connection Without Authentication | 286 |

Splunk returned **1,200 total SSH events** across four event categories.

---

## 4. Which Source IPs Generated Failed SSH Logins?

### SPL

```spl
index=ssh_logs source="ssh_logs.json" event_type="Failed SSH Login"
| stats count by id.orig_h
| head 10
```

### Finding

The search grouped failed SSH login attempts by source IP address. This helped identify systems that generated repeated failed authentication attempts.

### Interpretation

Source IPs with repeated failures may represent:

* Mistyped credentials
* Misconfigured scripts
* Password spraying
* Brute-force activity
* Unauthorized access attempts

The bar chart visualization made it easier to compare source IP activity quickly.

---

## 5. Which Hosts Were Involved In Multiple Failed Authentication Attempts?

### SPL

```spl
index=ssh_logs source="ssh_logs.json" event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h id.resp_h
```

### Finding

The search grouped repeated authentication failures by source and destination host.

### Interpretation

This event type is more suspicious than a single failed login because it already indicates repeated failed authentication behavior.

In a real SOC environment, this search could support brute-force triage by showing:

* Attacking or noisy source IP
* Targeted destination host
* Frequency of repeated failures
* Possible source-to-destination attack paths

---

## 6. Which Source IPs Successfully Logged In?

### SPL

```spl
index=ssh_logs source="ssh_logs.json" event_type="Successful SSH Login"
| stats count by id.orig_h id.resp_h
```

### Finding

The search grouped successful SSH login activity by source and destination host.

### Interpretation

Successful SSH logins are not automatically malicious, but they become important when compared against failed login patterns. A successful login after repeated failures may require investigation for possible account compromise.

Useful follow-up questions include:

* Did the same source IP generate failed and successful logins?
* Was the destination host expected?
* Was the login time normal for the user or system?
* Did the source IP appear in brute-force searches?

---

## 7. Which Source IPs Connected Without Authentication?

### SPL

```spl
index=ssh_logs event_type="Connection Without Authentication"
| stats count by id.orig_h
```

### Finding

The search counted SSH connections that did not complete authentication.

### Interpretation

Connections without authentication may indicate:

* SSH port scanning
* Service probing
* Interrupted sessions
* Automated reconnaissance
* Misconfigured tools

This activity should be reviewed when it repeats across many hosts or occurs from unusual source IPs.

---

## 8. How Did Unauthenticated SSH Activity Change Over Time?

### SPL

```spl
index=ssh_logs event_type="Connection Without Authentication"
| timechart count by id.orig_h
```

### Finding

The timechart grouped unauthenticated SSH connections by source IP over time.

### Interpretation

Time-based views help analysts identify bursts of activity, repeated probing, or coordinated scanning behavior.

---

# Alert Logic

The lab included alert logic for brute-force style SSH behavior.

### Example SPL

```spl
index=ssh_logs event_type="Multiple Failed Authentication Attempts"
| stats count by id.orig_h id.resp_h
| where count > 5
```

### Suggested Alert Settings

| Alert Setting | Value |
| --- | --- |
| Trigger Condition | More than 5 failed attempts |
| Time Window | 10 minutes |
| Severity | High |
| Investigation Focus | Source IP, destination host, repeated attempts |

---

# Key Findings

The investigation identified several useful SSH monitoring patterns:

* 1,200 total SSH events were analyzed.
* Splunk successfully parsed the JSON fields needed for investigation.
* 306 events were successful SSH logins.
* 305 events were failed SSH logins.
* 303 events were multiple failed authentication attempts.
* 286 events were connections without authentication.
* Failed login searches identified source IPs responsible for authentication failures.
* Multiple failed authentication attempts provided a useful brute-force detection path.
* Unauthenticated SSH connections may indicate scanning or probing behavior.

---

# Analyst Assessment

The dataset contains several patterns that would deserve review in a SOC environment.

Repeated failed authentication attempts and unauthenticated SSH connections can indicate brute-force activity, scanning, or reconnaissance. Successful logins should also be reviewed when they occur after failed attempts or from unusual source IPs.

The evidence in this lab supports suspicious activity detection practice, but it does not by itself prove compromise. In a real environment, the next step would be to correlate with user identity, endpoint telemetry, VPN logs, asset ownership, geolocation, threat intelligence, and change history.

---

# SPL Commands Practiced

During this project I practiced several SPL commands:

```spl
stats
```

Used to summarize event counts.

```spl
stats count by event_type
```

Used to count events by category.

```spl
stats count by id.orig_h
```

Used to group events by source IP.

```spl
stats count by id.orig_h id.resp_h
```

Used to compare source and destination relationships.

```spl
timechart
```

Used to visualize activity over time.

```spl
where
```

Used to filter results after aggregation.

---

# Screenshot Evidence

Twelve screenshots and one architecture image are included in the [evidence gallery](screenshots/README.md). They show the upload workflow, source type preview, index selection, successful ingestion, SPL statistics, and visualizations.

## Supporting Files

- [SPL searches](searches/investigation.spl)

The original `ssh_logs.json` dataset is not included in this repository.

---

# Skills Demonstrated

* Splunk Enterprise
* SPL
* SIEM log analysis
* SSH authentication monitoring
* Brute-force detection
* Failed login investigation
* Source IP analysis
* Destination host analysis
* Dashboard and visualization workflow
* Alert logic design
* SOC documentation

---

# What I Learned

This project reinforced that effective Splunk analysis starts with a clear security question.

The most useful workflow was:

**Ask what behavior matters → identify the fields → write the SPL → review the evidence → explain the security meaning.**

I also practiced using Splunk visualizations to make authentication patterns easier to understand and communicate.

---

# Disclaimer

This project uses lab-provided SSH log data for educational purposes. IP addresses, events, and activity patterns are part of a controlled training environment and do not represent production systems.

---

## Author

**Gabriel Akoi**

Cybersecurity Student | SOC Analyst Learner | Splunk & Security Monitoring
