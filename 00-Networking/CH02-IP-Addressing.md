# CH-02: IP Address

## Definition
**IP Address (Internet Protocol Address)** — A unique 
numerical label assigned to every device on a network 
so data knows where to go. Works like a home address 
for your computer.

## Analogy
- Your House → 192.168.1.5
- Friend's House → 192.168.1.8

## IPv4
- 32-bit address written as 4 numbers separated by dots
- Example: 192.168.1.5
- Range of each number: 0–255

## Two Types of IP

**Private IP** — IP address that only works inside 
a local network. Not accessible from internet directly.
- 192.168.0.0/16 → Home networks
- 172.16.0.0/12 → Medium organizations
- 10.0.0.0/8 → Large corporations

**Public IP** — IP address assigned by your internet 
provider. Visible to the entire internet.
- Example: Like your building's address

## Commands
```bash
ifconfig
# Look for inet 192.168.1.5 → this is your private IP
nmap -sn 192.168.1.0/24
# Scans entire subnet for live hosts
CIDR Subnet Sizes

	•	/24 = 254 hosts (most common)
	•	/16 = 65,534 hosts (large network)
	•	/32 = 1 host (single machine, firewall rules)
	•	/8 = 16M hosts (massive corporate)
	•	/28 = 14 hosts (small subnet, common in cloud)
	•	/22 = 1022 hosts (medium network)

Key Point

Pentest commonly scans entire /24 subnet for live hosts
