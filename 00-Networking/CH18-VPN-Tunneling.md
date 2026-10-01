# CH-18: VPN and Tunneling

## VPN (Virtual Private Network)
**Definition** — Creates an encrypted tunnel between your 
device and another network. Makes traffic appear to come 
from a different IP or location.

## tun0 Interface
A virtual network interface that appears in Kali Linux 
when connected to TryHackMe or Hack The Box via VPN.
Always use this IP in your payloads and listeners.

## VPN Commands
```bash
sudo openvpn yourfile.ovpn   # Connect to THM/HTB VPN
ip a                          # Verify connection (look for tun0)
ifconfig tun0                 # See your VPN IP specifically

Tunneling

The technique of encapsulating one protocol inside another
to bypass firewalls or security controls.

DNS Tunneling

Hiding data inside DNS queries to bypass firewalls.
Used by attackers for data exfiltration.

Port Forwarding

Redirecting traffic from one IP/port to another.
Used in pentesting to access internal services
through a compromised machine.

Key Point
TryHackMe/HTB → requires VPN to access lab machines
tun0 IP       → use this in reverse shells and exploits
Tunneling     → bypass firewall restrictions
