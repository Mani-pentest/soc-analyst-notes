# CH-12: Important Networking Commands

## Commands Reference

| Command | Purpose |
|---------|---------|
| ifconfig | Shows IP, MAC, network interfaces on Linux |
| ip a | Modern alternative to ifconfig |
| ip route | Shows routing table including gateway |
| ping | Tests if host is reachable |
| traceroute | Shows every router hop to destination |
| netstat -tulnp | Shows all open ports and listening services |
| arp -a | Displays ARP table (IP to MAC mappings) |
| nslookup | Queries DNS for IP of domain name |
| dig | Advanced DNS lookup — more powerful than nslookup |
| nmap | Network scanner for hosts and open ports |
| wireshark | Captures and analyzes network traffic |

## Practice Commands
```bash
ifconfig
ip a
ip route
ping -c 4 google.com
traceroute google.com
netstat -tulnp
arp -a
nslookup google.com
dig google.com
nmap -sV 192.168.1.1
