# CH-13: Nmap

## Definition
**Nmap (Network Mapper)** — A tool used to discover hosts, 
open ports, running services and vulnerabilities on a network. 
Most used tool in every pentest.

## Port States
| State | Meaning |
|-------|---------|
| Open | Service actively listening — primary target |
| Closed | Port accessible but no service running |
| Filtered | Firewall blocking — cannot determine state |
| Unfiltered | Accessible but open/closed unclear |

## Essential Commands

```bash
# Basic scan
nmap 192.168.1.1

# Scan entire subnet
nmap 192.168.1.0/24

# Service version detection
nmap -sV 192.168.1.1

# Stealth SYN scan (harder to detect)
nmap -sS 192.168.1.1

# Full TCP connect scan (louder, leaves logs)
nmap -sT 192.168.1.1

# UDP scan
nmap -sU 192.168.1.1

# OS detection
nmap -O 192.168.1.1

# Aggressive scan
nmap -A 192.168.1.1

# Scan ALL 65535 ports
nmap -p- 192.168.1.1

# Scan specific ports
nmap -p 80,443,445 192.168.1.1

# Run vulnerability scripts
nmap --script vuln 192.168.1.1

# THE scan for every CTF and real pentest
nmap -sV -sC -p- -T4 192.168.1.1

# Save results to file (always do in real engagements)
nmap -sV -oN output.txt 192.168.1.1
```

## Key Flags

| Flag | Purpose |
|---|---|
| `-sV` | Service version detection |
| `-sC` | Default scripts |
| `-p-` | All 65535 ports |
| `-T4` | Faster timing |
| `-Pn` | Skip ping (use when ICMP blocked) |
| `-oN` | Save output to file |
