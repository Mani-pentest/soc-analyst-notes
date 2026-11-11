# SOC Analyst Notes — Manikanta Rapolu

B.Tech Cybersecurity graduate in Hyderabad, preparing for a SOC Analyst (L1) role. This repo holds the structured notes I write while studying networking, Linux, Windows, security fundamentals, and the TryHackMe SOC Level 1 path. Each note has definitions, tables, commands, and a short summary. My hands-on project work is linked below.

## Start Here

If you only read a few files, read these:

| File | What it shows |
|---|---|
| [SOC09 — Windows Security Monitoring](06-SOC/SOC09-Windows-Security-Monitoring.md) | Event IDs (4624, 4625, 4720, 4732), Sysmon, RDP brute force, phishing delivery, persistence, and how to correlate with Logon ID and ProcessId |
| [SOC10 — Linux Security Monitoring](06-SOC/SOC10-Linux-Security-Monitoring.md) | auth.log, auditd and process trees, SSH brute force, discovery, persistence, and a Windows-to-Linux log map |
| [SOC05 — Phishing Analysis](06-SOC/SOC05-Phishing-Analysis.md) | Email header analysis, SPF/DKIM/DMARC, phishing tools, the analyze → investigate → remediate workflow |
| [SOC07 — Network Security Monitoring](06-SOC/SOC07-Network-Security-Monitoring.md) | Perimeter logs, scan detection, data exfiltration, man-in-the-middle, IDS and Snort rules |
| [SOC06 — Network Traffic Analysis](06-SOC/SOC06-Network-Traffic-Analysis.md) | Wireshark display filters, packet analysis, NetworkMiner, investigation workflow |

## Projects

| Project | Status |
|---|---|
| [Phishing Email Investigation](https://github.com/Mani-pentest/phishing-email-investigation): manual SOC L1 investigations of real phishing samples, with screenshots, defanged IOCs, MITRE ATT&CK mapping, and incident reports | 2 cases complete, Case 03 in progress |
| Home SIEM detection lab (Splunk) with detections mapped to MITRE ATT&CK | Planned |

## Credentials

| Credential | Status |
|---|---|
| Google Cybersecurity Professional Certificate (Coursera) | Completed |
| TryHackMe Pre-Security path | Completed |
| TryHackMe SOC Level 1 path | In progress (10 of 14 modules completed) |

## TryHackMe SOC Level 1 Progress

| Module | Notes | Status |
|---|---|---|
| Blue Team Introduction | [SOC01](06-SOC/SOC01-Blue-Team-Introduction.md) | Completed |
| SOC Team Internals | [SOC02](06-SOC/SOC02-SOC-Team-Internals.md) | Completed |
| Core SOC Solutions | [SOC03](06-SOC/SOC03-Core-SOC-Solutions.md) | Completed |
| Cyber Defence Frameworks | [SOC04](06-SOC/SOC04-Cyber-Defence-Frameworks.md) | Completed |
| Phishing Analysis | [SOC05](06-SOC/SOC05-Phishing-Analysis.md) | Completed |
| Network Traffic Analysis | [SOC06](06-SOC/SOC06-Network-Traffic-Analysis.md) | Completed |
| Network Security Monitoring | [SOC07](06-SOC/SOC07-Network-Security-Monitoring.md) | Completed |
| Web Security Monitoring | [SOC08](06-SOC/SOC08-Web-Security-Monitoring.md) | Completed |
| Windows Security Monitoring | [SOC09](06-SOC/SOC09-Windows-Security-Monitoring.md) | Completed |
| Linux Security Monitoring | [SOC10](06-SOC/SOC10-Linux-Security-Monitoring.md) | Completed |
| Malware Concepts for SOC | Notes coming | In progress |
| Threat Analysis Tools | Notes coming | Planned |
| SIEM Triage for SOC | Notes coming | Planned |
| SOC Level 1 Capstone Challenges | Notes coming | Planned |

New SOC notes are added after each TryHackMe module is finished.

## Contents

| Folder | Topics |
|---|---|
| [00-Networking](00-Networking) | IP addressing, ports, TCP/UDP, DNS, OSI, subnetting, ARP, DHCP, Nmap, Wireshark, firewalls, VPN, ICMP |
| [01-Linux](01-Linux) | Filesystem, permissions, processes, bash, networking, services, cron, logs, privilege escalation basics |
| [02-Windows](02-Windows) | Fundamentals, registry, Defender and BitLocker, privileges, internals |
| [03-Active-Directory](03-Active-Directory) | Domains, OUs, groups, GPO, Kerberos and NetNTLM |
| [04-Programming](04-Programming) | Python basics, automation and regex, bash scripting, JavaScript XSS concepts |
| [05-Security-Fundamentals](05-Security-Fundamentals) | CIA triad, risk, OWASP Top 10, malware types, encryption and hashing, web basics |
| [06-SOC](06-SOC) | Blue team, SOC internals, SOC tooling, frameworks, phishing analysis, network traffic analysis, and network, web, Windows, and Linux security monitoring |

## Tools Used

| Tool | Where I used it |
|---|---|
| Kali Linux, Nmap | Own VirtualBox lab |
| Wireshark, NetworkMiner, Snort | TryHackMe labs |
| Event Viewer, Sysmon, auditd | TryHackMe labs |
| Splunk, Elastic Stack | TryHackMe labs |
| CyberChef, PhishTool, Any.Run | TryHackMe labs |
| VirusTotal, urlscan.io, ICANN Lookup | Phishing Email Investigation project |

## Links

- TryHackMe: [tryhackme.com/p/manikantarapolu9](https://tryhackme.com/p/manikantarapolu9)
- GitHub: [github.com/Mani-pentest/soc-analyst-notes](https://github.com/Mani-pentest/soc-analyst-notes)
- Phishing project: [github.com/Mani-pentest/phishing-email-investigation](https://github.com/Mani-pentest/phishing-email-investigation)

## Contact

- Email: manikantarapolu97@gmail.com
- Location: Hyderabad, India
