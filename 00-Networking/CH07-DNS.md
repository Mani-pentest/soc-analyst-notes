# CH-07: DNS

## Definition

**DNS (Domain Name System)** — The internet's phonebook. Translates human-readable domain names into IP addresses that computers understand.

**DNS Server** — A server that stores domain name records and answers DNS queries from clients.

**DNS Query** — A request sent to a DNS server asking for the IP address of a domain.

## DNS Record Types

| Record | Purpose | Example |
|---|---|---|
| A | Hostname to IPv4 | google.com = 142.250.77.46 |
| AAAA | Hostname to IPv6 | The IPv6 equivalent of an A record |
| MX | Mail server | mail.gmail.com |
| CNAME | Alias/nickname | www.google.com → google.com |
| TXT | Miscellaneous text | Often contains security info (SPF, verification strings) |
| NS | Name server | Which server manages DNS for the domain |
| PTR | Reverse DNS | IP to hostname |

## How a DNS Lookup Works

| Step | What happens |
|---|---|
| 1 | The client checks its local DNS cache |
| 2 | If not cached, it asks a recursive resolver (for example the ISP's resolver or 8.8.8.8) |
| 3 | The resolver asks a root server, then the TLD server (for example .com) |
| 4 | The TLD server points to the authoritative name server for the domain |
| 5 | The authoritative server returns the record; the resolver passes it to the client and caches it for the record's TTL |

## Commands

| Command | Purpose |
|---|---|
| `nslookup google.com` | Basic DNS lookup |
| `dig google.com` | Advanced DNS lookup |
| `dig google.com MX` | Mail server records |
| `dig google.com TXT` | Text records |
| `dig google.com ANY` | Request all record types (many servers now limit this) |

```bash
nslookup google.com
dig google.com
dig google.com MX
dig google.com TXT
```

## DNS in Wireshark (SOC View)

| Filter | Purpose |
|---|---|
| `dns` | All DNS packets |
| `dns.flags.response == 0` | DNS queries |
| `dns.flags.response == 1` | DNS responses |
| `dns.flags.rcode == 3` | NXDOMAIN responses |
| `dns.qry.name contains "example"` | Queries whose name contains a string |

| Response code | Meaning |
|---|---|
| NOERROR | No DNS error; the record may or may not exist |
| NXDOMAIN | The domain name does not exist |
| SERVFAIL | The DNS server failed to complete resolution |

## Suspicious DNS Activity

- Many random-looking domain names
- Repeated queries in a short period
- Long, unusual subdomain names
- Large volumes of DNS requests
- Repeated NXDOMAIN responses

**Note:** These patterns can point to a Domain Generation Algorithm (DGA) or DNS tunneling, but legitimate software can cause them too. Suspicious DNS is an indicator to investigate, not proof of malware.

## Summary

- DNS translates domain names into IP addresses; the main record types are A, AAAA, MX, CNAME, TXT, NS and PTR
- A lookup goes client cache → recursive resolver → root → TLD → authoritative server, with the answer cached for its TTL
- `nslookup` and `dig` are the main lookup tools
- In Wireshark, `dns.flags.rcode == 3` finds NXDOMAIN responses, which are a common sign to check for DGA activity
