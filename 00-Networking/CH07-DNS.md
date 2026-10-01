# CH-07: DNS

## Definition
**DNS (Domain Name System)** — The internet's phonebook. 
Translates human-readable domain names into IP addresses 
that computers understand.

**DNS Server** — A server that stores domain name records 
and answers DNS queries from clients.

**DNS Query** — A request sent to a DNS server asking 
for the IP address of a domain.

## DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| A | Hostname to IPv4 | google.com = 142.250.77.46 |
| AAAA | Hostname to IPv6 | - |
| MX | Mail server | mail.gmail.com |
| CNAME | Alias/nickname | www.google.com → google.com |
| TXT | Miscellaneous text | Often contains security info |
| NS | Name server | Which server manages DNS |
| PTR | Reverse DNS | IP to hostname |

## Commands
```bash
nslookup google.com      # Basic DNS lookup
dig google.com           # Advanced DNS lookup
dig google.com MX        # Mail server records
dig google.com TXT       # Text records
dig google.com ANY       # Give me everything
