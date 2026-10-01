# CH-09: Subnetting

## Definition
**Subnet** — A smaller logical division of a larger network.

## Analogy
Imagine a large apartment building:
- Building = 192.168.1.0/24
- Floors = subnets
- Flats = individual IP addresses

## What /24 Means
192.168.1.0/24
- 24 bits are fixed (the network part)
- 8 bits are free (for devices)
- 2^8 = 256 addresses
- 256 - 2 = 254 usable hosts
- First = network address, Last = broadcast

## Key Subnet Sizes to Memorize
| CIDR | Hosts | Use Case |
|------|-------|----------|
| /24 | 254 | Most common — almost every CTF |
| /16 | 65,534 | Large network |
| /8 | 16M | Massive corporate |
| /28 | 14 | Small — common in cloud |
| /30 | 2 | Point-to-point links |
| /32 | 1 | Single machine |
| /22 | 1022 | Medium network |

## Commands
```bash
ipcalc 192.168.1.0/24
ipcalc 10.0.0.0/8
ipcalc 172.16.0.0/28
# ipcalc shows network address, broadcast and all hosts

Network Address

First IP in a subnet. Represents the network itself.
Cannot be assigned to a device.

Broadcast Address

Last IP in a subnet. Sends data to all devices simultaneously.
Cannot be assigned to a device.
