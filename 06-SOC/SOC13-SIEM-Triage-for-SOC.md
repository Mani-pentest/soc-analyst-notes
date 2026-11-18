# SOC13: SIEM Triage for SOC

Covers how an L1 analyst uses a SIEM to triage alerts: why a SIEM helps, which log sources matter, how to search them, and how to follow an alert from first signal to a verdict. The module has five rooms. Notes cover the three learning rooms: Log Analysis with SIEM, Alert Triage With Splunk, and Alert Triage With Elastic. ItsyBitsy and Benign are challenge rooms and are not written up.

## Log Analysis with SIEM

A SIEM collects logs from many systems into one place so an analyst can search, correlate, and review them without logging in to each device.

| Benefit | What It Means |
|---|---|
| Centralisation | Logs from endpoints, network devices, and applications are searched in one place |
| Correlation | Separate events are linked to build the full story, for example an IDS alert plus the Windows logs of the host behind it |
| Historical events | Past logs show whether an activity is new or part of a pattern, such as a login from a new location |

| Log Source | Examples | What It Helps Detect |
|---|---|---|
| Host-based | Windows event logs, Sysmon, Linux auth.log and syslog | Process execution, logins, account changes, persistence |
| Network-based | Firewall, router, IDS and IPS logs | Scans, unusual connections, traffic patterns |
| Web-based | Apache and Nginx access and error logs | Brute force, web shells, DDoS, scanning |

**Note:** Time zones differ across sources. Logs may be in UTC, local time, or have no zone, and the SIEM may show a different time from yours. A gap between your clock and the log time is not a delay in ingestion.

**Note:** Normalisation converts logs from different formats (JSON, XML, plain text) into one set of field names, which makes searching and correlating easier.

| Windows Source | Event | What It Shows |
|---|---|---|
| Sysmon | EventCode 1 | Process creation, including parent and command line |
| Sysmon | EventCode 3 | Network connections made by a process |
| Security log | 4720, 4722 | User account created, account enabled |
| System log | 7045, 7036 | Service created, service started or stopped |

| Linux and Web Source | What It Shows |
|---|---|
| auth.log | SSH logins, failed passwords, sudo and su use |
| syslog | Cron jobs, services, system events, often persistence |
| Web access log | Repeated POSTs to a login page, requests to script files, error 503 floods |

| Search Pattern in Splunk | Purpose |
|---|---|
| `index=<index> EventCode=1 powershell EncodedCommand` | Find encoded PowerShell launches |
| `index=<index> EventCode=3 ComputerName=<host>` | List network connections from one host |
| `index=<index> EventCode=4720 OR EventCode=4722` | Find new or enabled accounts |
| `index=<index> EventCode=7045 OR EventCode=7036` | Find new services and state changes |
| `source="auth.log" process=sshd "Accepted password" OR "Failed password"` | Compare failed and successful SSH logins |
| `sourcetype=syslog CRON` | Find scheduled-job persistence on Linux |

**Note:** A web brute-force alert is confirmed by grouping POST requests to the login page by source IP over a short time window. Check the user-agent for automated tools.

## Alert Triage With Splunk

This room gives three alerts and a Splunk instance. Each alert is a short scenario: read the alert, search the right index, decide whether it is real, and report what the attacker did.

| Scenario | What You Investigate |
|---|---|
| Initial access alert | A Linux host: did a brute-force attack succeed, and what happened after access |
| Persistence alert | A Windows host: a scheduled task was created, is it legitimate or an attacker's foothold |
| Web shell alert | A vulnerable web server: how a malicious file was planted and used |

| Triage Step | Action |
|---|---|
| Read the alert | Note the alert name, time, host, user, and the detection logic |
| Scope the search | Pick the matching index and set the time range around the alert |
| Find the trigger | Search for the event that caused the alert |
| Look before and after | Check what happened in the minutes around it, such as repeated failures before a login |
| Follow the attacker | Look for privilege escalation, new accounts, and new tasks or files |
| Decide | True positive or false positive, with the evidence |
| Report | Write a short summary and escalate to L2 if it is real |

| Alert Type | What the Logs Should Tell You |
|---|---|
| Brute force | Number of failures, source IP, time span, and the first success after the failures |
| Privilege escalation | A switch from a normal user to root or admin, and which account did it |
| Account persistence | A newly created user, who created it, and when |
| Scheduled task | Task name, creating user, what it runs, and whether the path is unusual |
| Web shell | An unexpected script file, the requests that reached it, and the source IP and user-agent |

**Note:** A single event is rarely enough. The verdict comes from the sequence: access, escalation, then persistence.

## Alert Triage With Elastic

The same workflow in the Elastic Stack. You use Kibana to read logs, find indicators of compromise, and connect events from different log sources as a series of alerts.

| Stage in the Room | What You Look For |
|---|---|
| Investigating web attacks | Suspicious requests and scanning in web logs |
| Uncovering account activity | New accounts and unusual logins linked to the attacker |
| Exposing command execution | Process creation and command lines run after the attacker gained access |

| Kibana Pattern | Purpose |
|---|---|
| `process.name: "whoami.exe"` | Match one process by name using a field and value |
| `process.name: "whoami.exe" AND event.category: "process"` | Combine conditions to narrow results |

**Note:** The Discover tab is where analysts spend most of their time in Kibana: search bar, time range, filters, and the field list.

| Item | Splunk | Elastic |
|---|---|---|
| Query language | SPL, commands joined with pipes | KQL (Kibana Query Language), field and value pairs |
| Main search screen | Search and Reporting | Discover |
| Strength in this module | Log search and statistics | Filtering and viewing logs by field |

**Note:** Do not confuse the two KQLs. Kibana Query Language (Elastic) is different from Kusto Query Language (Microsoft Sentinel). Both are called KQL.

## Alert Triage Workflow

| Step | Action |
|---|---|
| Receive | Take ownership of the alert so another analyst does not duplicate it |
| Read | Note the rule logic, host, user, source, and time |
| Search | Query the SIEM around the alert time and host |
| Correlate | Add a second source such as endpoint or web logs |
| Enrich | Check IPs, hashes, and domains with threat intel |
| Decide | True positive, false positive, or needs more data |
| Document | Record the timeline and evidence |
| Escalate or close | Pass a confirmed incident to L2 with a clear summary |

## Common Mistakes

- Judging an alert from one event instead of the surrounding sequence
- Forgetting the time zone difference between logs and the SIEM
- Searching only the alert host and missing the source IP that attacked it
- Calling a brute force successful before confirming a login after the failures
- Closing a scheduled task alert without checking what the task actually runs
- Writing a report with no timeline or evidence

## Module Takeaway

Log Analysis with SIEM explains why a SIEM matters (centralisation, correlation, history) and which logs answer which question: Sysmon and Windows event IDs, Linux auth and syslog, and web access logs. Alert Triage With Splunk and Alert Triage With Elastic put it into practice: read the alert, search around it, follow the attacker from access to escalation to persistence, then decide and escalate. The skill is the same on any SIEM, only the query syntax changes.
