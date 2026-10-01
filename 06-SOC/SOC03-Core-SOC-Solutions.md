# SOC03: Core SOC Solutions

Covers the three core technology categories a SOC analyst works with daily: EDR (endpoint-level visibility), SIEM (centralized log visibility, covering both Splunk and Elastic Stack), and SOAR (orchestration and automation on top of both). Five rooms: Introduction to EDR, Introduction to SIEM, Splunk: The Basics, Elastic Stack: The Basics, Introduction to SOAR.

## EDR (Endpoint Detection and Response)

EDR monitors behavior on the endpoint itself, rather than relying only on a known-malware signature list.

| Component | Role |
|---|---|
| Agent | Installed on the endpoint, collects telemetry |
| Console | Central dashboard where agent data is reviewed |

| Detection Type | Description |
|---|---|
| Signature-based | Matches against known-bad indicators |
| Behavior-based | Flags suspicious patterns even if never seen before |

**Note:** The key differentiator over traditional antivirus is **visibility** — EDR sees process creation, file access, registry changes, and network activity, not just a static file list. A process tree, where every child process links back to its parent, is how an analyst spots something like `WINWORD.EXE` spawning `powershell.exe`. EDR is host-centric and is paired with SIEM, NDR, email gateways, IAM, and DLP for full defense-in-depth — it does not replace network-level controls.

## SIEM (Security Information and Event Management)

A SIEM centralizes logs from across an entire environment into one searchable platform, replacing the need to check dozens of individual machines separately.

### Splunk

| Component | Role |
|---|---|
| Forwarder | Lightweight agent on the endpoint, collects and sends data |
| Indexer | Processes and stores the data received from forwarders |
| Search Head | Where analysts search, using SPL (Search Processing Language); results return as field-value pairs |

**Note:** The default Splunk app is **Search & Reporting**.

### Elastic Stack (ELK)

Originally built for data storage, search, and visualization; now widely used in SOCs as a lightweight SIEM.

| Component | Role |
|---|---|
| Beats | Lightweight agents on hosts (e.g., Filebeat for logs, Winlogbeat for Windows events, Packetbeat for network data) |
| Logstash | Data processing layer — ingests, filters, and forwards data |
| Elasticsearch | Full-text search and analytics engine; stores and indexes data |
| Kibana | Web-based visualization layer; search, investigate, and build dashboards in real time |

**Data flow:** Beats collect → Logstash processes → Elasticsearch stores → Kibana visualizes.

**Note:** Kibana's **Discover** tab is the main workspace for viewing and filtering logs, using **KQL (Kibana Query Language)** — the Elastic equivalent of Splunk's SPL. Kibana runs on port 5601 by default.

### Splunk vs Elastic — Same Shape, Different Names

| Role | Splunk | Elastic |
|---|---|---|
| Collect | Forwarder | Beats |
| Process/Store | Indexer | Logstash + Elasticsearch |
| Search/Display | Search Head | Kibana |

## SOAR (Security Orchestration, Automation, and Response)

SOAR unifies SIEM, EDR, firewalls, IAM, and ticketing systems into a single interface, built specifically to address **alert fatigue** — the volume problem a traditional SOC faces when alerts outpace what analysts can manually handle.

| Capability | Description |
|---|---|
| Orchestration | Coordinating multiple separate security tools together so they work as one system |
| Automation | SOAR executes a predefined playbook itself, with no manual clicking required |
| Playbook | A predefined list of actions to handle a specific incident type |

**Worked example:** a VPN brute force alert triggers a playbook — SOAR receives the alert from the SIEM, and can automatically block the source IP on the firewall, disable the affected user in IAM, and open a ticket, all without manual intervention. Playbooks can also branch: if the source IP matches the user's normal pattern and failed attempts were minimal, the playbook may stop early instead of escalating.

**Note:** Automation does not remove the analyst from the loop — manual analysis remains vital within a SOAR workflow, particularly for ambiguous cases a playbook isn't built to handle. This is the direct precursor to the AI-driven SOC automation discussed in current industry coverage: SOAR is the established, rule-based version of what AI SOC agents are now extending further.

**Section takeaway:** EDR watches the endpoint. SIEM (Splunk or Elastic) centralizes and searches logs from everywhere. SOAR sits on top of both, connecting tools and automating the repeatable response actions — freeing analyst time for the judgment calls a playbook can't make.
