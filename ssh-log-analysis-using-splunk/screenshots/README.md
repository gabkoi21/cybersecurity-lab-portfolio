# Screenshot Evidence

These screenshots document the SSH log analysis workflow in Splunk Enterprise.

| Image | Evidence |
| --- | --- |
| [architecture-overview.jpg](architecture-overview.jpg) | Lab overview showing SSH log sample ingestion into Splunk |
| [01-add-data-options.png](01-add-data-options.png) | Splunk Add Data page with upload, monitor, and forward options |
| [02-upload-ssh-logs.png](02-upload-ssh-logs.png) | `ssh_logs.json` selected and uploaded successfully |
| [03-source-type-preview.png](03-source-type-preview.png) | JSON event preview with extracted SSH fields |
| [04-input-settings-index.png](04-input-settings-index.png) | Input settings showing the `ssh_logs` index |
| [05-review-upload.png](05-review-upload.png) | Review page showing file name, source type, host, and index |
| [06-upload-success.png](06-upload-success.png) | Splunk confirmation that the file was uploaded successfully |
| [07-event-type-counts.png](07-event-type-counts.png) | SPL statistics showing counts by SSH event type |
| [08-failed-login-chart.png](08-failed-login-chart.png) | Failed SSH login visualization by source IP |
| [09-multiple-failed-attempts.png](09-multiple-failed-attempts.png) | Multiple failed authentication attempts by source and destination |
| [10-successful-login-stats.png](10-successful-login-stats.png) | Successful SSH login activity by source and destination |
| [11-connection-without-authentication.png](11-connection-without-authentication.png) | Connections without authentication grouped by source IP |
| [12-timechart-unauthenticated-connections.png](12-timechart-unauthenticated-connections.png) | Timechart of unauthenticated SSH connections by source IP |

## Architecture Overview

![SSH log analysis architecture overview](architecture-overview.jpg)

## Add Data Workflow

![Splunk Add Data page showing upload, monitor, and forward options](01-add-data-options.png)

## Upload SSH Logs

![Splunk upload page showing ssh_logs.json selected and uploaded successfully](02-upload-ssh-logs.png)

## Source Type Preview

![Splunk source type preview showing parsed JSON SSH fields](03-source-type-preview.png)

## Input Settings

![Splunk input settings showing the ssh_logs index selected](04-input-settings-index.png)

## Review Upload Settings

![Splunk review page showing upload settings before submission](05-review-upload.png)

## Upload Confirmation

![Splunk confirmation showing file upload completed successfully](06-upload-success.png)

## Event Type Counts

![Splunk statistics showing SSH event type counts](07-event-type-counts.png)

The search returned 1,200 total events across four event categories: successful logins, failed logins, multiple failed authentication attempts, and connections without authentication.

## Failed Login Visualization

![Splunk bar chart showing failed SSH logins by source IP](08-failed-login-chart.png)

## Multiple Failed Authentication Attempts

![Splunk statistics table showing multiple failed authentication attempts by source and destination](09-multiple-failed-attempts.png)

## Successful SSH Logins

![Splunk statistics table showing successful SSH logins by source and destination](10-successful-login-stats.png)

## Connections Without Authentication

![Splunk statistics table showing SSH connections without authentication by source IP](11-connection-without-authentication.png)

## Timechart of Unauthenticated Connections

![Splunk timechart showing unauthenticated SSH connections by source IP](12-timechart-unauthenticated-connections.png)

## Evidence Scope

The screenshots come from a local Splunk Enterprise lab using a provided SSH log dataset. The original `ssh_logs.json` dataset is not included in this repository.
