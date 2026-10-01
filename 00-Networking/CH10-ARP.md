# CH-10: ARP

## Definition
**ARP (Address Resolution Protocol)** — A protocol used 
to discover the MAC address of a device when only its 
IP address is known. Operates at Layer 2.

## How It Works

Your PC broadcasts to EVERYONE:
“Hey! Who has IP 192.168.1.8? Tell me your MAC!”

Device with 192.168.1.8 replies:
“That’s me! My MAC is AA:BB:CC:DD:EE:FF”

Your PC saves this in ARP table and communicates directly.

## ARP Table
```bash
arp -a
# Shows IP to MAC address mappings cached on your device
ARP has a fatal flaw — NO VERIFICATION.
Any device can lie and say “I have that IP.”

ARP Poisoning Attack

You trick devices into sending all their traffic
to you instead of the real destination.
You become the Man in the Middle.

Attack Example:

	•	You tell Device A: “I am the router” (fake MAC response)
	•	You tell Router: “I am Device A” (fake MAC response)
	•	All traffic now flows through you = MITM

Command

arpspoof -i eth0 -t 192.168.1.5 192.168.1.1
