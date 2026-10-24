# CH-14: Wireshark

## Definition

**Wireshark** — A graphical tool that captures and analyzes network traffic passing through a network interface in real time.

**Packet Capture** — Intercepting and recording network packets as they travel across a network.

**Display Filter** — A filter applied to show only specific traffic from packets already captured. It hides packets; it does not delete them.

**Capture Filter** — A filter set before capturing that decides which packets are recorded at all. Packets that do not match are dropped permanently.

## Key Display Filters

| Filter | Purpose |
|---|---|
| `tcp` | Show only TCP traffic |
| `udp` | Show only UDP traffic |
| `dns` | Show all DNS traffic |
| `http` | Show HTTP traffic Wireshark recognizes |
| `ip.addr == 192.168.1.5` | Traffic to or from a specific IP |
| `ip.src == 192.168.1.5` | Traffic sent by a specific IP |
| `tcp.port == 80` | Traffic on a specific port |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Initial SYN packets (connection attempts) |
| `http.request.method == "POST"` | HTTP POST requests (for example form submissions) |

**Note:** Type filters with plain straight quotes. Curly quotes copied from documents make Wireshark reject the filter.

## Combining Filters

| Operator | Meaning |
|---|---|
| `&&` | AND, both conditions must match |
| `\|\|` | OR, either condition can match |
| `!` | NOT, exclude matching packets |

```
ip.src == 192.168.1.5 && tcp
dns || http
!dns
```

## Use Cases (SOC View)

| Use case | What to look for |
|---|---|
| Spot a possible port scan | Many SYN packets from one source to many different ports in a short time |
| Review DNS activity | Repeated NXDOMAIN responses, very long subdomains |
| Find cleartext credentials | Logins over HTTP, FTP or Telnet, where the data is readable |
| See the TCP three-way handshake | SYN, then SYN-ACK, then ACK |
| Understand what a host talks to | Statistics > Conversations |

**Note:** HTTPS content is encrypted, so Wireshark shows connection metadata, not the page contents, unless you have the decryption keys.

## Summary

- Wireshark captures and inspects packets; a display filter only hides packets, a capture filter permanently drops them
- Core display filters: `ip.addr`, `tcp.port`, `dns`, `http`, and TCP flag filters
- Combine filters with `&&`, `||` and `!`
- A filter finds packets; analysis explains the behavior; correlated evidence supports the conclusion
- Full SOC investigation workflow: see SOC06 (Network Traffic Analysis)
