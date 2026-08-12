# Splunk: The Basics — Full Walkthrough

## 1. Lab Overview

This TryHackMe lab introduced the core features of Splunk and provided practical experience ingesting and investigating VPN logs. The objective was to upload a newline-delimited JSON dataset, store it in a dedicated index, validate the imported events, and answer investigation questions with SPL.

## 2. What Is Splunk?

Splunk Enterprise is a platform used to ingest, index, search, analyze, and visualize machine-generated data. Security teams can use it as part of a Security Information and Event Management (SIEM) workflow to centralize logs, monitor security activity, investigate alerts, and support incident response.

Example data sources include:

- Operating-system and authentication logs
- Endpoint and application activity
- Web server and firewall logs
- VPN and remote-access logs
- Business-system events, such as registrations or transaction alerts

## 3. Core Splunk Components

### Indexer

The indexer processes incoming data, transforms it into searchable events, and stores it in indexes. An index is a logical collection of data rather than simply a folder or traditional relational database.

### Forwarder

A forwarder is a lightweight Splunk component installed on or near a data source. It collects data and sends it to an indexer for processing and storage.

### Search Head

The search head provides the interface used to run SPL searches, create reports, build dashboards, and examine results. In distributed environments, it coordinates searches across indexers.

## 4. Data Ingestion

Ingestion is the process of bringing data into Splunk. Depending on the environment, Splunk can receive data through:

- Manual file uploads
- Splunk forwarders
- Syslog
- Network inputs such as TCP or UDP
- APIs and other integrations

In this lab, I manually uploaded a newline-delimited JSON file named `VPN_logs`. Each line represented an individual VPN event.

## 5. Important Event Metadata

Splunk commonly uses the following metadata to identify and organize events:

| Field | Meaning |
| --- | --- |
| `index` | The logical data store searched for matching events |
| `source` | The file, stream, or input from which the event originated |
| `sourcetype` | The classification describing the data's format and structure |
| `host` | The host value associated with the event or input |

These are searchable metadata fields, not the four basic SPL commands.

## 6. Splunk Interface

The lab introduced several areas of the Splunk interface:

- **Splunk bar:** Global navigation and account controls
- **Apps panel:** Access to installed Splunk applications
- **Explore Splunk:** Links to learning and discovery features
- **Dashboards:** Visual displays of searches, metrics, and trends
- **Search & Reporting:** The primary workspace for writing SPL and investigating events

## 7. Import Workflow

I used the following process to ingest the VPN dataset:

1. Opened **Add Data** from the Splunk home page.
2. Selected **Upload** and chose the local `VPN_logs` file.
3. Kept the JSON source type detected by Splunk.
4. Configured the input and created or selected the `VPN_Logs` index.
5. Reviewed the configuration and completed the upload.
6. Opened **Search & Reporting** and changed the time range to **All time**.

The **All time** setting was important because a narrower time window could exclude events and produce misleading counts.

## 8. Search Processing Language

Search Processing Language (SPL) is Splunk's query language. It is used to retrieve events and transform results through a pipeline of commands. SPL shares some analytical concepts with SQL, but its syntax and event-processing model are distinct.

The pipe character (`|`) passes the results of one stage to the next. For example, a base search can retrieve events, `spath` can extract JSON fields, and `stats` can aggregate the final results.

## 9. Validation and Investigation Searches

### Confirm the total number of imported events

```spl
index=VPN_Logs
| stats count
```

This query verifies that the dataset was ingested and returns the number of indexed events.

### Count events associated with the user Maleena

```spl
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count
```

### Identify the username associated with a source IP address

```spl
index=VPN_Logs
| spath
| search Source_ip="107.14.182.38"
| stats values(UserName) AS UserName count
```

Using `values(UserName)` returns the distinct usernames observed for the selected IP address. The accompanying count helps show how many matching events were found.

### Count events originating outside France

```spl
index=VPN_Logs
| spath
| search Source_Country!="France"
| stats count
```

This search excludes events whose extracted `Source_Country` value is France. In a production investigation, I would also check for missing country values because an inequality filter can behave differently depending on field existence.

A more explicit validation search is:

```spl
index=VPN_Logs
| spath
| where isnotnull(Source_Country) AND Source_Country!="France"
| stats count
```

### Count events associated with a second source IP address

```spl
index=VPN_Logs
| spath
| search Source_ip="107.3.206.58"
| stats count
```

## 10. JSON Field Extraction

If fields such as `UserName`, `Source_ip`, or `Source_Country` do not appear automatically, the following command parses JSON content at search time:

```spl
| spath
```

For example:

```spl
index=VPN_Logs
| spath
| table _time UserName Source_ip Source_Country
```

This table provides a quick way to confirm that the expected fields exist and contain usable values before performing aggregation.

## 11. Findings and Result Integrity

The supplied completion screenshot confirms the following results:

| Investigation question | Verified result |
| --- | ---: |
| Total events in the `VPN_Logs` index | **2,862** |
| Events associated with `Maleena` | **60** |
| Username associated with `107.14.182.38` | **Smith** |
| Events originating from countries other than France | **2,814** |
| Events associated with `107.3.206.58` | **14** |

Each value is shown as a correct answer in the supplied TryHackMe evidence. The SPL searches in the preceding section provide the reproducible method used to obtain those results.

One supplied Splunk screenshot displays a separate `index=windowslogs` search containing 12,256 Windows events. That image demonstrates familiarity with the Search & Reporting interface, but it is not evidence for the VPN dataset and is therefore kept separate from the VPN findings.

## 12. Skills Developed

- Navigating the Splunk interface
- Understanding indexer, forwarder, and search-head responsibilities
- Uploading newline-delimited JSON data
- Creating and searching a dedicated index
- Extracting JSON fields with `spath`
- Filtering by username, IP address, and country
- Aggregating results with `stats`
- Validating time ranges, event counts, and field extraction

## 13. Lessons Learned

This lab showed that reliable SIEM analysis begins before the investigation query is written. The source type, index, host metadata, time range, and field extraction must all be correct for results to be trustworthy. I also learned to validate imported data with a broad count before applying narrower filters and to preserve the exact SPL used so findings can be reproduced.

## 14. Reference

- [SPL cheat sheet supplied with the lab notes](https://l1nk.dev/89xchek)

The shortened link could not be independently verified while preparing this portfolio entry. It is retained only as a user-supplied learning resource.

## 15. Training Disclaimer

This walkthrough documents a controlled TryHackMe training exercise. It does not represent production SOC employment or analysis of a live organization's data. Protected room answers, credentials, flags, and fabricated evidence are excluded.
