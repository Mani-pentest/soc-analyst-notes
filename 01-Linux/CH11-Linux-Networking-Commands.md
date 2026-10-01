# CH-11: Linux Networking Commands

## Overview
Commands used to view/change network configuration, view connectivity, and transfer data from the command line.

## Definitions
| Command | Definition |
|---------|------------|
| `ip a` | Shows all interfaces assigned IP addresses (modem) |
| `ifconfig` | Same purpose, legacy command |
| `ss -tulnp` | Shows active connections using each port |
| `restart -tulnp` | Same purpose, and the process using each |

## ss Flag Breakdown
`ss` is a command used to show which machine connections on your ports are open (active connections, etc).

| Flag | Meaning |
|------|---------|
| `-t` | TCP connections |
| `-u` | UDP connections |
| `-l` | Listening ports only |
| `-n` | Numeric IP (no name resolution) |
| `-p` | Shows the process using the connection |

**Example:** `ss -tulnp`
→ Show me all TCP & UDP ports that are listening, and tell me which process owns each one.

## ping / traceroute
- `ping chost>` → Test connectivity to host
- `ping -c 4 <host>` → Limit ping by 4 attempts instead of running indefinitely.

## curl / wget
| Command | Definition |
|---------|------------|
| `curl <url>` | Download a server's content from a URL |
| `curl -O <url>` | Download and save the content as a file |
| `curl <url>` | Fetch and display content fully, running indefinitely |
| `wget <url>` | Download a file from a URL |

## Summary
- `ip a`/`ifconfig` shows network configuration
- `ss` shows listening ports; `curl`/`wget` display and download content from a URL
- Ping tests reachability; curl/wget test download files
