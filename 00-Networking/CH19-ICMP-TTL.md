# CH-19: ICMP and TTL

## ICMP (Internet Control Message Protocol)

**Definition** — A network protocol used for diagnostic and error reporting. No port number — operates directly over IP. Ping uses ICMP.

## ICMP Types

| Type | Purpose |
|------|---------|
| Echo Request | Ping sends this to target |
| Echo Reply | Target responds confirming alive |
| Unreachable | Destination not reachable |
| Time Exceeded | TTL expired (used in traceroute) |

## TTL (Time To Live)

**Definition** — A number embedded in every network packet. Decreases by 1 at each router hop. When TTL reaches 0 the packet is discarded. Prevents packets looping forever.

## TTL OS Fingerprinting

| OS | Starting TTL |
|---|---|
| Windows | 128 |
| Linux | 64 |
| Cisco | 255 |

- Ping a target and get TTL around 64 → likely Linux/Unix
- TTL around 128 → likely Windows

**Note:** TTL is a hint, not proof. It drops by 1 at every hop and the starting value can be changed.

## Why Pentesters Care

| Situation | Meaning |
|---|---|
| Ping fails | Firewall may be blocking ICMP |
| Solution | `nmap -Pn` (skip ping, scan directly) |
| TTL value | Reveals operating system type |
| Traceroute | Maps network infrastructure |

## Commands

```bash
ping -c 4 192.168.1.1     # Test if host is alive
traceroute google.com      # Map network path
nmap -Pn 192.168.1.1      # Scan even if ping fails
```
