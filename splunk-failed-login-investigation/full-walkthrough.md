# Splunk Failed Login Investigation

## Project Overview

This project demonstrates a beginner-level SOC investigation using Splunk to analyze authentication activity and identify suspicious failed login patterns.

The goal was to investigate login events, identify repeated failures, determine which users and systems were targeted, review source IP activity, and check whether failed login attempts were followed by successful authentication.

The investigation was completed in a local Splunk Enterprise lab environment using a custom CSV authentication dataset.

---

## Objective

The main objectives of this project were to:

* Ingest authentication logs into Splunk.
* Identify failed and successful login activity.
* Determine which user accounts experienced the most failures.
* Identify source IP addresses responsible for failed attempts.
* Determine which endpoints were targeted.
* Detect repeated login failures occurring within a short period.
* Investigate privileged account activity.
* Identify successful logins following failed attempts.
* Determine whether the same IP address was associated with both failed and successful authentication.
* Practice writing and understanding basic SPL queries.

---

## Lab Environment

**SIEM:** Splunk Enterprise
**Environment:** Local SOC lab
**Data Source:** CSV authentication logs
**Total Events:** 15

### Dataset Fields

The dataset included the following fields:

* `timestamp`
* `user`
* `src_ip`
* `dest_ip`
* `host`
* `action`
* `event_id`
* `logon_type`
* `source`
* `status`

Windows-style authentication event IDs were used:

* `4624` — Successful login
* `4625` — Failed login

---

# Investigation Workflow

I followed a question-driven investigation approach:

**Question → Evidence Needed → Fields → SPL → Result → Interpretation**

This helped me focus on what I was trying to discover instead of randomly running SPL commands.

---

## 1. How Many Login Events Were Ingested?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv"
| stats count
```

### Finding

Splunk returned:

**15 total authentication events**

---

## 2. How Many Logins Failed vs Succeeded?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv"
| stats count by action
```

### Finding

* Failed logins: **11**
* Successful logins: **4**

Failed authentication represented the majority of the events in the dataset.

---

## 3. Which Users Experienced Failed Login Attempts?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv" action="failed"
| stats count by user
```

### Finding

| User          | Failed Attempts |
| ------------- | --------------: |
| admin         |               6 |
| administrator |               2 |
| john          |               2 |
| mary          |               1 |

The `admin` account received the highest number of failed login attempts.

---

## 4. Which Source IPs Generated Failed Logins?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv" action="failed"
| stats count by src_ip
| sort - count
```

### Finding

| Source IP    | Failed Attempts |
| ------------ | --------------: |
| 192.168.1.25 |               5 |
| 203.0.113.50 |               3 |
| 10.10.5.44   |               2 |
| 192.168.1.15 |               1 |

The highest volume of failed attempts came from `192.168.1.25`.

---

## 5. Which Systems Were Targeted?

Because Splunk used `socagent` as the metadata host, the endpoint hostname extracted from the CSV appeared as `extracted_host`.

### SPL

```spl
index="main" host="socagent" action="failed"
| stats count by extracted_host
| sort - count
```

### Finding

* `WIN-PC01` — 10 failed attempts
* `WIN-PC02` — 1 failed attempt

Most failed login activity was directed toward `WIN-PC01`.

---

## 6. Were Failed Logins Happening Close Together?

### SPL

```spl
index="main" host="socagent" action="failed"
| table timestamp user src_ip
```

### Finding

A clear cluster was observed against the `admin` account:

* 08:08:19 — `admin` — `203.0.113.50`
* 08:08:33 — `admin` — `203.0.113.50`
* 08:08:48 — `admin` — `203.0.113.50`

This represents **three failed attempts within 29 seconds**.

Another short sequence occurred against the `administrator` account from `10.10.5.44`.

---

## 7. Were Privileged Accounts Targeted?

### SPL

```spl
index="main" sourcetype="csv" action="failed" (user="admin" OR user="administrator")
| stats count by user
```

### Finding

* `admin` — 6 failed attempts
* `administrator` — 2 failed attempts

Repeated attempts against privileged accounts warranted additional investigation.

---

## 8. Why Did Authentication Fail?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv" event_id=4625
| stats count by user status
```

### Finding

The failure reasons included:

* Invalid password
* Unknown username
* Account disabled

The `admin` account generated six invalid-password failures.

---

## 9. Did A Successful Login Occur After Failed Attempts?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv" (event_id="4624" OR event_id="4625")
| table user timestamp status
```

### Finding

The user `john` showed the following sequence:

* 08:02:01 — Unknown username
* 08:06:20 — Invalid password
* 08:09:14 — Login successful

A successful login therefore occurred after previous failed attempts.

---

## 10. Did The Successful Login Come From The Same IP?

### SPL

```spl
index="main" source="splunk_failed_login_lab.csv" user="john" (event_id="4624" OR event_id="4625")
| table user timestamp status src_ip
```

### Finding

All three events associated with `john` came from:

`192.168.1.25`

This means the same source IP generated both failed attempts and the later successful login.

This could represent a legitimate user eventually entering the correct password, but in a real SOC environment the activity would require additional context before reaching a final conclusion.

---

# Key Findings

The investigation identified several notable authentication patterns:

* 15 total login events were analyzed.
* 11 were failed login attempts.
* 4 were successful logins.
* The `admin` account received the highest number of failures.
* `WIN-PC01` was the primary targeted endpoint.
* `203.0.113.50` generated three failed attempts against `admin` within 29 seconds.
* Privileged accounts received repeated authentication failures.
* Several failure reasons were observed, including invalid passwords and disabled accounts.
* `john` experienced failed attempts followed by a successful login.
* The failed and successful attempts associated with `john` originated from the same IP address.

---

# Analyst Assessment

The activity contains several patterns that would deserve additional investigation in a real SOC environment.

Repeated authentication failures against privileged accounts, rapid failed attempts from the same source, and a successful authentication following failed attempts are useful indicators for further triage.

However, authentication failures alone do not prove malicious activity.

Additional evidence such as endpoint logs, VPN logs, geographic information, user confirmation, device history, threat intelligence, and authentication baselines would be required before determining whether an account was compromised.

---

# SPL Commands Practiced

During this project I practiced several foundational Splunk commands:

```spl
stats
```

Used to summarize events.

```spl
stats count by user
```

Used to count events for each user.

```spl
table
```

Used to display selected fields clearly.

```spl
sort
```

Used to order results.

```spl
search
```

Used to filter events.

```spl
head
```

Used to limit displayed results.

---

# Screenshot Evidence

Six supplied screenshots are included in the [evidence gallery](screenshots/README.md). They show raw events, login outcomes, failures by user and source IP, detailed failed events, and John's authentication sequence. The detailed event table supports the endpoint counts and rapid failure timeline; separate screenshots of endpoint aggregation and privileged-account aggregation were not supplied.

## Supporting Files

- [SPL searches](searches/investigation.spl)
- [Original SPL cheat sheet](resources/Splunk_Failed_Login_SPL_Cheat_Sheet.pdf)

The original CSV was not supplied and is not included. Queries below use the source name visible in the screenshots. Findings are documented from the supplied write-up and screenshot evidence; searches have not been rerun against a live Splunk instance.

---
# Skills Demonstrated

* Splunk Enterprise
* SPL
* SIEM log analysis
* Authentication monitoring
* Failed login investigation
* SOC alert triage
* Event correlation
* Timeline analysis
* Source IP analysis
* Account activity analysis
* Incident documentation

---

# What I Learned

This project helped me understand that Splunk investigation is not mainly about memorizing SPL commands.

A better workflow is:

**Ask a security question → identify the evidence needed → identify the fields → build the SPL → review the results → interpret the activity.**

While completing the project, I used Splunk documentation and AI assistance to validate some SPL syntax while learning. I executed the searches in my own Splunk environment and reviewed each result to understand why the search worked and what the evidence meant.

---

# Disclaimer

This project uses simulated authentication data created for educational purposes. IP addresses, usernames, hosts, and events do not represent real production systems or users.

---

## Author

**Gabriel Akoi**

Cybersecurity Student | SOC Analyst Learner | Splunk & Security Monitoring

