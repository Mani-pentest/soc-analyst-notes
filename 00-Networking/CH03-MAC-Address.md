# CH-03: MAC Address

## Definition
**MAC Address (Media Access Control)** — A permanent 
physical address burned into your network card by the 
manufacturer at the factory. Never changes unlike IP address.

## Analogy
- IP Address = Your current hotel room number (changes)
- MAC Address = Your passport number (permanent)

## Format
6 pairs of hexadecimal numbers
- Example: 00:1A:2B:3C:4D:5E
- First 3 bytes → Manufacturer (OUI)
- Last 3 bytes → Unique device ID

## Command
```bash
ifconfig
# Look for line that says ether → that's your MAC address

Pentesting Relevance

	•	MAC spoofing — change your MAC to bypass
network access controls
	•	ARP poisoning — fake MAC addresses to intercept traffic
	•	OUI lookup — identify device manufacturer on network
