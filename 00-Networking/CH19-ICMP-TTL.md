# CH-19: ICMP and TTL

## ICMP (Internet Control Message Protocol)
**Definition** — A network protocol used for diagnostic 
and error reporting. No port number — operates directly 
over IP. Ping uses ICMP.

## ICMP Types
| Type | Purpose |
|------|---------|
| Echo Request | Ping sends this to target |
| Echo Reply | Target responds confirming alive |
| Unreachable | Destination not reachable |
| Time Exceeded | TTL expired (used in traceroute) |

## TTL (Time To Live)
**Definition** — A number embedded in every network packet. 
Decreases by 1 at each router hop. When TTL reaches 0 
the packet is discarded. Prevents packets looping forever.

## TTL OS Fingerprinting
Windows → TTL starts at 128
Linux   → TTL starts at 64
Cisco   → TTL starts at 255

If you ping a target and get TTL 64 → likely Linux/Unix
If TTL is around 128 → likely Windows


## Why Pentesters Care
If ping fails   → firewall may be blocking ICMP
Solution        → nmap -Pn (skip ping, scan directly)
TTL value       → reveals operating system type
Traceroute      → maps network infrastructure

## Commands
```bash
ping -c 4 192.168.1.1     # Test if host is alive
traceroute google.com      # Map network path
nmap -Pn 192.168.1.1      # Scan even if ping fails
