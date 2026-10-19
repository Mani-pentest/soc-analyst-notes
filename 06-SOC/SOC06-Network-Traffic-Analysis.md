# SOC06: Network Traffic Analysis

Covers how a SOC analyst reads network traffic: which attack patterns to look for, how Wireshark display filters work, how to use Wireshark's statistics views, how to spot specific attacks in a capture, and how NetworkMiner extracts evidence from a PCAP. Five rooms: Network Traffic Basics, Wireshark: The Basics, Wireshark: Packet Operations, Wireshark: Traffic Analysis, NetworkMiner.

## Network Traffic Basics

Network Traffic Analysis (NTA) is capturing, inspecting, and analyzing data as it flows across a network. It combines packet inspection, log correlation, and network flow statistics, rather than only watching individual packets.

| Technique | What It Looks Like | Notes |
|---|---|---|
| DNS tunneling | Many DNS queries to subdomains of one domain from a single host | Hides command-and-control (C2) traffic inside DNS, which is rarely blocked |
| Session hijacking | An unexpected jump in the TCP sequence number | Suggests another source is injecting itself into an existing session |
| IDS evasion by fragmentation | Malicious traffic split across multiple packets | The attacker hopes the IDS fails to reassemble and inspect the full payload |

**Note:** HTTPS traffic is encrypted with TLS (Transport Layer Security). In an ordinary capture an analyst can see IP addresses, ports, timing, and some handshake details, but not the content.

## Wireshark: The Basics

Wireshark is an open-source, cross-platform packet analyzer used to investigate live or captured (PCAP) traffic.

| Feature | Purpose |
|---|---|
| Capture File Properties (Statistics menu) | File metadata, including any comments left by whoever captured it |
| Packet numbers | A unique number per packet, which keeps large captures trackable |
| Go to Packet | Jump directly to a specific packet number |
| Find Packets | Search by display filter, hex value, string, or regex |
| Follow Stream | Right-click a packet, then Follow, then TCP or HTTP Stream, to reconstruct the full conversation |
| Export of raw bytes | Reconstruct a file embedded in a capture, then verify it with a hash such as `md5sum` |

Packet details are dissected by OSI layer: Layer 3 (IP, including TTL), Layer 4 (TCP, including flags, ports, and payload size), and the application layer (for example HTTP).

## Display Filter Logic

A capture is a table: each packet is a row and each protocol field is a column (`ip.src`, `ip.dst`, `tcp.dstport`, `dns.qry.name`). A display filter asks that table a question in the form **field, operator, value**, for example `tcp.port == 443`.

- **Finding a field name:** click the field in the packet details and the status bar shows its name. Right-click, then Apply as Filter, and Wireshark writes the filter. The filter bar is green when valid and red when broken.
- **Layer prefixes:** `ether`, `ip`, `ip6`, `arp`, `tcp`, `udp`, and application protocols such as `dns`, `http`, `ftp`. Typing only a protocol name shows every packet of that protocol.
- **Direction:** `ip.addr` matches either side, while `ip.src` and `ip.dst` are directional.
- **Operators:** `==`, `!=`, `>`, `<`, `>=`, `<=`, `contains`, `matches` (regex), `in {...}`.
- **Combining:** `&&` means AND, `||` means OR, and `!` negates. Use brackets to group conditions.
- **Display filter vs capture filter:** a display filter only hides packets from view. A capture filter decides what is collected, and a wrong one can miss evidence.

| Goal | Filter |
|---|---|
| Traffic to or from an IP | `ip.addr == 10.0.0.5` |
| Traffic from or to an IP only | `ip.src == 10.0.0.5` / `ip.dst == 10.0.0.5` |
| One port | `tcp.port == 443` |
| DNS queries / responses | `dns.flags.response == 0` / `dns.flags.response == 1` |
| HTTP requests | `http.request` |
| POST requests | `http.request.method == "POST"` |
| Connection attempts (SYN) | `tcp.flags.syn == 1 && tcp.flags.ack == 0` |
| SYN-ACK | `tcp.flags.syn == 1 && tcp.flags.ack == 1` |
| TCP reset | `tcp.flags.reset == 1` |

| Service | Common Port |
|---|---|
| DNS | UDP 53 |
| HTTP / HTTPS | TCP 80 / 443 |
| SSH | TCP 22 |
| FTP | TCP 21 (data on 20) |
| SMTP | TCP 25, 587, 465 |
| DHCP | UDP 67 and 68 |
| NTP | UDP 123 |

**Note:** A port number is a clue about a service, not proof of what the traffic contains. Services can use nonstandard ports, and a filter finds matching packets without classifying them.

## Wireshark: Packet Operations

The Statistics menu gives the big picture before any single packet is inspected.

| Statistics View | What It Shows |
|---|---|
| Resolved Addresses | IPs and hostnames in the capture, showing which domains were reached |
| Protocol Hierarchy | A tree of protocols with packet counts and percentages, useful for spotting odd protocols |
| Conversations | Traffic between two endpoints, in Ethernet, IPv4, IPv6, TCP, and UDP views |
| Endpoints | The unique devices, with extras such as MAC vendor and location |
| Protocol-specific stats | IPv4/IPv6, DNS (queries, responses, codes), and HTTP (requests, responses) |

**Note:** If you can click on a field, you can filter on it. Right-click, then Apply as Filter, turns any value you see into a filter.

## Wireshark: Traffic Analysis

This room applies filters to recognise attacks inside raw captures. Each task follows the same pattern: filter to the protocol, then look for the telltale sign.

| Attack | What to Look For | Filter or Step |
|---|---|---|
| Nmap TCP connect scan | One source sending SYNs to many ports, with handshakes completed | `tcp.flags.syn == 1 && tcp.flags.ack == 0 && tcp.window_size > 1024` |
| Nmap SYN scan | SYNs to many ports where the handshake is never completed | The same SYN filter, then check which connections never finish |
| UDP scan | Closed UDP ports answer with an ICMP unreachable message | `icmp` |
| ARP poisoning / MITM | Two MACs claiming the same IP, or one MAC flooding ARP requests | `arp.duplicate-address-detected or arp.duplicate-address-frame`, `arp.opcode == 1` (requests), `arp.opcode == 2` (replies) |
| Host identification | DHCP, NetBIOS, and Kerberos packets reveal hostnames and usernames | Filter the protocol, then add the field of interest as a column |
| DNS tunneling | Long, random-looking subdomains under one domain | `dns.qry.name.len > 15 and !mdns`, or `dns contains "dnscat"` |
| ICMP tunneling | Pings carrying unusually large payloads | `icmp`, then check the data length |
| FTP | Cleartext credentials; many failed logins suggest brute force | `ftp`, then look for response code 530 (login failed) |
| HTTP | Odd user agents and exploit patterns such as Log4j | Check `http.user_agent` |
| HTTPS | Encrypted until the key log is loaded | Edit, Preferences, Protocols, TLS, (Pre)-Master-Secret log filename, then filter `http2` |

**Note:** DNS and ICMP tunneling are hard to block because both protocols are needed for normal network operation. The room's bonus tasks cover hunting for cleartext credentials and turning findings into firewall rules.

**Reasoning about a possible port scan:** many initial SYN packets from one source to many ports in a short time may indicate scanning. It does not prove that connections succeeded, that the source is malicious, or that the server is compromised. Check the responses (SYN-ACK, RST, or none), the time span, and whether an approved vulnerability scan was running.

## NetworkMiner

NetworkMiner is a network forensics tool. Loading a PCAP makes it extract the artifacts automatically, instead of reading packet by packet.

| Tab | What It Shows |
|---|---|
| Hosts | IP, MAC, OS fingerprint, open ports, and sessions per host |
| Sessions | Individual connections |
| DNS | DNS queries and responses |
| Credentials | Cleartext logins and password hashes |
| Files and Images | Extracted files and images, which can be downloaded |
| Messages | Emails and other messages |
| Parameters, Keywords | Extracted parameters and keyword searches |
| Anomalies | Limited anomaly detection |

**Note:** Wireshark and NetworkMiner complement each other. Wireshark gives packet-level detail, while NetworkMiner gives a fast overview of hosts, files, and credentials. NetworkMiner is not an IDS. Extracted hashes can be cracked offline with Hashcat or John the Ripper.

## Investigation Workflow

1. Statistics, then Protocol Hierarchy and Conversations, to see what is in the capture.
2. Filter to the suspect host or protocol.
3. Inspect the packet details.
4. Follow the TCP or HTTP stream to read the whole conversation.
5. Check whether each request received a response.
6. Write the finding as Observation, Evidence, Assessment, and Next step.

## Common Mistakes

- Confusing source and destination, or `ip.addr` with `ip.src` and `ip.dst`.
- Treating a port as proof of a protocol.
- Treating a connection attempt (SYN) as a completed connection.
- Calling something unusual malicious without corroborating evidence.
- Assuming HTTPS content is readable without decryption keys.
- Treating one missing response as proof, since it may be blocked, lost, or outside the capture.

## Module Takeaway

Network Traffic Basics identifies the attacks worth looking for. The three Wireshark rooms build the tool skills in order: navigation and Follow Stream, then statistics and filter logic, then recognising attack patterns. NetworkMiner is the fast route to hosts, files, and credentials in a capture. When an alert points to network activity, Wireshark confirms what happened and NetworkMiner extracts the evidence quickly.
