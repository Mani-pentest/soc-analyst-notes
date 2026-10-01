# CH-17: Firewall, IDS and IPS

## Firewall
**Definition** — A security system that monitors and controls 
incoming and outgoing network traffic based on predetermined 
security rules.

### Types
| Type | Function |
|------|----------|
| Packet Filtering | Checks IP/port rules |
| Stateful | Tracks connection state |
| Application | Inspects application data |
| WAF (Web App Firewall) | Protects web apps |

## IDS (Intrusion Detection System)
**Definition** — Monitors network traffic for suspicious 
activity and generates alerts when attack detected.
- Does NOT block traffic — only detects and alerts.

## IPS (Intrusion Prevention System)
**Definition** — Same as IDS but actively blocks detected 
threats in real time. Both detects AND prevents.

## Why Pentesters Care
Loud scans (nmap -T5)    → trigger IDS alerts
Stealth scans (-sS, -T2) → quieter, harder to detect
Some firewalls block ICMP → ping won’t work
Solution: nmap -Pn       → skip ping, scan anyway

## Evasion
The technique of bypassing firewalls and IDS systems 
without triggering alerts during a pentest engagement.
