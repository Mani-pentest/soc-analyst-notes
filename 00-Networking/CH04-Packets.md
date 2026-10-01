# CH-04: Packets

## Definition
**Packet** — A small chunk of data. Large data is broken 
into multiple packets before traveling across a network. 
Each packet travels independently and reassembles 
at destination.

## Analogy
Sending a 100-page book through WhatsApp:
- Book gets split into 100 pages
- Each page is put in a separate envelope
- Each envelope has:
  - Where it's going (destination IP)
  - Where it came from (source IP)
  - Which page number it is (sequence number)
- They all travel separately and reassemble at destination

## Components

**Packet Header** — Contains source IP, destination IP, 
sequence number and other control information.

**Packet Payload** — The actual data being carried 
inside the packet.

## Key Point
This is exactly how data travels on a network.
