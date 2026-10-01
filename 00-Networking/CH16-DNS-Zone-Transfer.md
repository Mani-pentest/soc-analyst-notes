# CH-16: DNS Zone Transfer

## Definition
**DNS Zone Transfer** — A process where a DNS server shares 
its complete database of records with another server. 
A misconfigured DNS server allows anyone to request this, 
giving attackers every subdomain, hostname and IP in 
the organization.

## Why It Matters
First thing checked in every external pentest. A successful 
zone transfer reveals the entire internal network structure.

## Command
```bash
dig axfr @nameserver target.com

# Example:
dig axfr @ns1.target.com target.com

What a Successful Zone Transfer Gives You

	•	All subdomains (mail, vpn, admin, dev, staging)
	•	Internal IP addresses
	•	Server hostnames
	•	Complete DNS database of the organization

Key Point

This is one of the first things you check in
every external pentest engagement.
