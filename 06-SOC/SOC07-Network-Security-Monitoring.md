# SOC07: Network Security Monitoring

Covers how a SOC analyst monitors the network perimeter and recognizes the footprints of network-based attacks in logs and packet captures. Six rooms: Network Security Essentials, Network Discovery Detection, Data Exfiltration Detection, Man-in-the-Middle Detection, IDS Fundamentals, Snort.

## Network Security Essentials

The network perimeter is the boundary between trusted (internal) and untrusted (outside) networks, and usually the first place an attacker touches. Monitoring it means using firewalls, IDS/IPS, and access control, and reading the logs they produce.

| Term | Meaning |
|---|---|
| Network visibility | Being able to see traffic and logs across the network |
| Baseline | What normal traffic looks like, so anomalies stand out |
| Host-centric logs | From endpoints (Windows event logs, auth logs) |
| Network-centric logs | From network devices (firewall, IDS/IPS, WAF, VPN, DNS) |

| Perimeter Log Source | What It Shows | What to Look For |
|---|---|---|
| Firewall | Connections allowed or denied | One source hitting many ports or hosts (scan) |
| IDS / IPS | Alerts with a reason (signature) | Scan, C2 beaconing, exfiltration, SMB alerts |
| WAF | Blocked web attacks (SQLi, XSS) | One source IP behind many blocks |
| VPN gateway | Login success or failure, assigned internal IP | Many failures on one username, then a success |

**Note:** An IDS alert explains why traffic was flagged, which makes it more useful than a plain firewall block. IDS detects and alerts; IPS sits inline and blocks.

The room's breach scenario follows a typical attack chain through the logs:

| Stage | What It Looks Like in the Logs |
|---|---|
| Reconnaissance | Many BLOCK events from one external IP across ports and hosts |
| Initial access | VPN brute force, then a success and an internal IP assigned |
| Lateral movement | SMB attempts on port 445 between internal hosts |
| C2 beaconing | Regular outbound connections from one internal host |
| Exfiltration | Large or unusual outbound transfer from one host |

| Command | Purpose |
|---|---|
| `head firewall.log` | See the log format first |
| `grep "BLOCK" firewall.log` | Show blocked connections |
| `grep "<IP>" firewall.log \| grep "ALLOW"` | Check whether the suspect IP got anything through |
| `... \| sort \| uniq -c \| sort -nr` | Count and rank sources or targets |

**Note:** A BLOCK means that attempt failed. An ALLOW from the same source is the one to chase, and no single log tells the whole story, so correlate firewall, IDS, and VPN logs.

## Network Discovery Detection

Network discovery is finding live hosts, open ports, and services. Attackers do it first to map the attack surface. Defenders also scan to reduce that surface, so a scan alone does not prove an attack. Check who owns the source IP and whether the scan was authorized.

| Scan Type | How It Works | What It Looks Like in Logs |
|---|---|---|
| Ping sweep | ICMP sent to many hosts to find which are alive | One source, ICMP, many destination IPs (often sequential) |
| TCP SYN scan | SYN sent to ports; a SYN-ACK reply means open. The handshake is never completed | Many TCP connection attempts with no reply; Zeek `conn_state` = S0 |
| UDP scan | Empty UDP packets sent; a closed port returns ICMP port unreachable | UDP traffic with ICMP unreachable replies. Slow and unreliable |

| Pattern | Meaning |
|---|---|
| Horizontal scan | One source, same destination port, many destination IPs |
| Vertical scan | One source, one destination IP, many destination ports |

**Note:** Scanning from outside is reconnaissance. Scanning that starts inside the network points to either a compromised host or an authorized vulnerability scanner, and both need verifying. The room used Kibana with Zeek logs; useful fields are `network.protocol`, `source.ip`, `destination.ip`, and `zeek.conn.conn_state`.

MITRE ATT&CK: T1046 (Network Service Discovery) and T1018 (Remote System Discovery).

## Data Exfiltration Detection

Data exfiltration is the unauthorized transfer of data from an organization to an external destination controlled by an adversary. It can be an insider or malware using compromised credentials. The room covers four network channels, using Wireshark and Splunk.

| Channel | Indicators | Where to Look |
|---|---|---|
| DNS tunneling | Very long query names (encoded data in subdomains), many queries to one domain, high volume from one host | Wireshark: filter `dns`, check query lengths. Splunk: long query names |
| FTP | Large payloads to an external IP, logins from guest or root accounts, STOR and RETR commands | Wireshark: `ftp \|\| ftp-data` |
| HTTP | Large POST requests to an unusual external host | Wireshark: `http.request.method == "POST"` |
| ICMP | Echo packets with large or odd payloads | Wireshark: `icmp`, check data length |

**Note:** For passive FTP, correlate the control connection with a data connection on a high port followed by a large transfer. FTP sends commands in cleartext, so packet inspection shows what was transferred.

Triage questions: what data left, how much, where to, from which host or account, and what do proxy, DNS, flow, and host logs say about the same event.

MITRE ATT&CK: Exfiltration tactic (TA0010), including T1048 (Exfiltration Over Alternative Protocol).

## Man-in-the-Middle Detection

A man-in-the-middle (MITM) attack secretly intercepts or alters communication between two endpoints. The room chains three techniques in one scenario.

| Attack | How It Works | Indicators | Filters |
|---|---|---|---|
| ARP spoofing | Fake ARP replies tie the attacker's MAC address to a legitimate IP, usually the gateway. Works because ARP has no authentication | Many unsolicited (gratuitous) ARP replies; duplicate MACs or IPs | `arp`, `arp.opcode == 1` (requests), `arp.opcode == 2` (replies), `arp.isgratuitous`, `arp.duplicate-address-detected` |
| DNS spoofing | An attacker already on the network answers the victim's DNS query with a forged reply | Conflicting DNS responses for the same domain; a reply pointing to an unexpected IP | `dns` |
| SSL stripping | HTTPS is downgraded to HTTP so credentials travel in plaintext | A login POST over HTTP to a site that should use HTTPS | `tls \|\| ssl`, `http.request.method == "POST"` |

**Note:** On a host, `arp -a` showing two different IPs with the same MAC is a quick check. Defences include static ARP entries for the gateway, dynamic ARP inspection on switches, and HSTS to prevent HTTPS downgrades.

MITRE ATT&CK: T1557 (Adversary-in-the-Middle), including T1557.002 (ARP Cache Poisoning).

## IDS Fundamentals

An Intrusion Detection System (IDS) monitors traffic and alerts on suspicious activity. It does not stop the threat; an IPS does that.

| Deployment | Monitors |
|---|---|
| HIDS (host-based) | One host |
| NIDS (network-based) | Traffic across the whole network |

| Detection Method | How It Works | Strength | Weakness |
|---|---|---|---|
| Signature-based | Matches traffic against a database of known attack patterns | Fast, accurate for known attacks | Cannot detect zero-day attacks |
| Anomaly-based | Learns a normal baseline and flags deviations | Can detect zero-day attacks | Many false positives; needs tuning |
| Hybrid | Uses signatures for known threats and anomaly detection for unknown ones | Broader coverage | More processing and tuning |

**Note:** Signature-based detection suits a small threat surface. Anomaly-based and hybrid systems suit larger organizations facing zero-day attacks.

Snort is a widely used open-source IDS, first released in 1998. Its main mode is NIDS mode, which compares live traffic with rules and raises alerts. It can also log packets to a PCAP file. Custom rules go in `local.rules`.

## Snort

Snort has four operating modes: sniffer, packet logger, IDS/IPS, and PCAP processing.

| Command | Purpose |
|---|---|
| `snort -c /etc/snort/snort.conf -T` | Test the configuration file for errors |
| `snort -c /etc/snort/snort.conf -A console` | Run in IDS mode and print alerts to the terminal |
| `snort -c local.rules -A full -l .` | Run with a rule file, full alerts, logs in the current directory |
| `snort -c local.rules -r file.pcap -A console` | Run rules against a recorded PCAP |

| Option | Purpose |
|---|---|
| `-c` | Configuration or rule file |
| `-r` | Read a PCAP file |
| `-l` | Log directory |
| `-A` | Alert mode: console, full, fast, cmg, none |
| `-T` | Test configuration |
| `-N` | Disable logging |
| `-D` | Run in the background |

**Note:** In full, fast, and none modes nothing prints to the console. Check the alert file in the log directory instead.

A Snort rule has two parts: the rule header (action, protocol, source IP and port, direction, destination IP and port) and the rule options in parentheses.

```
alert tcp any any -> any 80 (msg:"HTTP traffic detected"; sid:1000001; rev:1;)
```

| Option Group | Examples |
|---|---|
| General | `msg` (alert text), `sid` (unique rule ID), `rev` (revision number) |
| Payload | `content`, `nocase`, `fast_pattern` |
| Non-payload | `flags` (F = FIN, S = SYN, R = RST), `id` (IP ID), `dsize` (payload size) |
| Post-detection | What happens after a match, such as logging |

**Note:** Custom rules go in `/etc/snort/rules/local.rules`, and user-created sids should start at 1,000,000 to avoid clashing with built-in rules. The header is required and the options are optional. Common syntax errors are a missing port (use `any`) and an invalid direction operator such as `<-`; use `->` or `<>`.

## Investigation Workflow

| Step | Action |
|---|---|
| Identify the stage | Recon, access, lateral movement, C2, or exfiltration |
| Pick the right log | Firewall for scans, VPN for logins, IDS for C2 and exfiltration, packet capture for ARP, DNS, and HTTP |
| Find the footprint | Scan pattern, long DNS queries, duplicate MACs, large outbound transfer |
| Verify | Check whether the source is an authorized scanner or normal service |
| Correlate and report | Match across logs, then write the finding |

## Common Mistakes

- Treating a firewall BLOCK as the end of the story, without checking whether the same source was allowed elsewhere
- Calling every scan malicious without checking for authorized vulnerability scanning
- Assuming a signature-based IDS will catch an unknown attack
- Writing a Snort rule with a missing port or the wrong direction operator and not testing it with `-T` or a PCAP

## Module Takeaway

Network Security Essentials gives the perimeter log sources and the stages of a breach. Network Discovery, Data Exfiltration, and Man-in-the-Middle Detection teach the specific footprints of three attack types, which are scan patterns, covert exfiltration channels, and ARP, DNS, and SSL manipulation. IDS Fundamentals and Snort add the detection tooling, covering how signature and anomaly detection work and how to write and run a rule. When a network alert arrives, identify the attack stage, find its footprint in the right log, verify the source, and correlate before concluding.
