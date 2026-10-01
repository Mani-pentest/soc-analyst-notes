# CH-14: Wireshark

## Definition
**Wireshark** — A graphical tool that captures and analyzes 
all network traffic passing through your network interface 
in real time.

**Packet Capture** — Intercepting and recording network 
packets as they travel across a network.

**Display Filter** — A filter applied to show only specific 
types of traffic from captured packets.

## Key Filters for Pentesting
tcp                           → Show only TCP traffic
udp                           → Show only UDP traffic
http                          → Show HTTP requests/responses
dns                           → Show all DNS queries
ip.addr == 192.168.1.5        → Traffic to/from specific IP
tcp.port == 80                → Traffic on specific port
tcp.flags.syn == 1            → Show SYN packets
http.request.method == “POST” → Find login form submissions

## Pentesting Use Cases
- Capture credentials on HTTP, FTP, Telnet (cleartext)
- See 3-way handshake in real traffic visually
- Analyze what services a target machine is communicating with
- Find POST requests containing credentials in cleartext
