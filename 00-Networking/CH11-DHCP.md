# CH-11: DHCP

## Definition
**DHCP (Dynamic Host Configuration Protocol)** — A protocol 
that automatically assigns IP addresses and network 
configuration to devices when they connect to a network.

## How It Works
Your PC:     “I just connected! Can I have an IP please?”
DHCP Server: “Sure! Take 192.168.1.15,
Your Gateway is 192.168.1.1,
Your DNS is 8.8.8.8”
Your PC:     “Thanks! I’ll use this for 24 hours”

## DHCP Server
Usually your router. Manages and distributes 
IP addresses to devices on the network.

## Why Pentesters Care
- DHCP gives you the Gateway IP → target for ARP poisoning
- DHCP gives you the DNS server → target for DNS attacks
- Rogue DHCP attack → set up fake DHCP server,
  give victims YOUR IP as gateway
  → all traffic goes through you

## Rogue DHCP Attack
Set up a fake DHCP server that gives victims 
a malicious gateway IP redirecting all their 
traffic through the attacker.
