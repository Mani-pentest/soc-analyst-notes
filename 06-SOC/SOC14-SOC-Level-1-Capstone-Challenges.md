# SOC14: SOC level 1 capstone challenges

The capstone has four challenge rooms: Tempest, Boogeyman 1, Boogeyman 2, and Boogeyman 3. Each one gives an incident and the artefacts, and the analyst has to rebuild what happened using skills from the whole path. These notes cover the approach and the tools for each room. They contain no answers.

## What the capstone tests

Every room follows the same line of work: find the entry point, follow the attacker step by step, and describe the impact. The data changes (emails, PowerShell logs, Sysmon logs, packet captures, memory images, Kibana), but the questions stay the same: what ran, who ran it, where did it connect, and what did it change.

## Tempest

A workstation was hit by a full attack chain. The investigation starts from a critical alert about a malicious document. The data is a Sysmon log, a Windows event log, and a packet capture.

- Start from the alert and find the document and the process it launched.
- Follow child processes in the Sysmon log to see what the document started.
- Check the packet capture for the connections that match those processes.
- Look for discovery commands, new accounts, and persistence.
- Finish with what the attacker did at the end of the chain.

| Tool | Purpose |
|---|---|
| Event Viewer | Read the Sysmon and Windows event logs |
| Wireshark | Follow the network traffic in the capture |

## Boogeyman 1

A phishing email leads to PowerShell activity on a workstation and data leaving over the network. The room has three parts: email analysis, endpoint logs, and network traffic.

- Email: read the headers, then inspect the attachment without running it.
- Endpoint: search the PowerShell logs for the commands the attachment started.
- Network: find the traffic that shows data leaving the network.

| Tool | Purpose |
|---|---|
| Thunderbird | Open the phishing email safely |
| lnkparse3 | Read the details inside a shortcut file |
| jq | Search and filter the JSON PowerShell logs |
| tshark and Wireshark | Read the packet capture |
| grep, sed, awk, base64 | Filter text and decode encoded commands |

## Boogeyman 2

A spear phishing email targets a department with a malicious document. The artefacts are a copy of the email and a memory dump of the victim's workstation.

- Document: pull out the macro code and read what it downloads and runs.
- Memory: list processes and command lines to find what the macro started.
- Persistence: look for how the attacker made the access survive a restart.

| Command | Purpose |
|---|---|
| `olevba document.doc` | Extract the macros from an Office document |
| `vol -f memorydump.raw -h` | List the Volatility plugins |
| `vol -f memorydump.raw <plugin>` | Run one plugin against the memory image |

Note: Volatility can take a few minutes to read the memory image the first time, so wait before assuming a plugin failed.

## Boogeyman 3

The data is in Elastic, with Sysmon logs from the victim network. The intrusion begins when a user runs a file and grows into a wider compromise.

- Use Kibana Discover and set the time window around the incident.
- Filter by host and user, then sort by time to build a timeline.
- Follow the stages in the order of the kill chain: execution, command and control, privilege escalation, credential theft, movement to other hosts, and final impact.

## What carried over from the rest of the path

- Phishing analysis: email headers, attachments, and shortcut files.
- Windows and Linux logs: process creation, network connections, and PowerShell logs.
- Network analysis: filtering a capture to the host and the time of the incident.
- Malware concepts: macros, encoded commands, and persistence.
- SIEM triage: building a timeline in Kibana.
- MITRE ATT&CK: mapping each step to a tactic.

## Common mistakes

- Reading only the final alert instead of tracing back to the first event
- Forgetting to decode a base64 command before judging it
- Ignoring the time window and searching all time
- Skipping the packet capture because the endpoint logs looked complete
- Writing conclusions without the log line that supports them

## Module takeaway

The capstone shows the whole analyst workflow on four incidents. Start from the alert or the email, find the first malicious action, follow the process and network trail, and write down the impact with evidence. Each room used different tools, but the method did not change.
