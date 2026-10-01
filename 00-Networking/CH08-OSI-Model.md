# CH-08: OSI Model

## Definition
**OSI Model (Open Systems Interconnection)** — A conceptual 
framework that divides network communication into 7 layers. 
Each layer has a specific job.

## The 7 Layers

| Layer | Name | What It Does | Data Unit |
|-------|------|-------------|-----------|
| 7 | Application | What user sees (browser, email) | Data |
| 6 | Presentation | Encryption, compression (SSL/TLS) | Data |
| 5 | Session | Manages connections between devices | Data |
| 4 | Transport | TCP/UDP, ports live here | Segment |
| 3 | Network | IP addresses live here | Packet |
| 2 | Data Link | MAC addresses live here | Frame |
| 1 | Physical | Cables, WiFi signals, hardware | Bits |

## Memory Trick
**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
- A = Application (7)
- P = Presentation (6)
- S = Session (5)
- T = Transport (4)
- N = Network (3)
- D = Data Link (2)
- P = Physical (1)

## Attacks Mapped to Layers
| Layer | Attack |
|-------|--------|
| Layer 7 | SQL Injection, XSS, CSRF (web attacks) |
| Layer 6 | SSL Stripping |
| Layer 5 | Session Hijacking |
| Layer 4 | Port Scanning, SYN Flood |
| Layer 3 | IP Spoofing |
| Layer 2 | ARP Poisoning, MAC Spoofing |
| Layer 1 | Physical cable tapping |

EXAMPLE-

## Letter Analogy
- You write the letter → L7 (App)
- Translate to common language → L6 (Presentation)
- Put in envelope, seal it → L5 (Session)
- Choose courier service → L4 (Transport)
- Write address, choose route → L3 (Network)
- Hand to local postman → L2 (Data Link)
- Physical delivery by van → L1 (Physical)
