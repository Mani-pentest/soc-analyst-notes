# CH-05: Ports

## Definition
**Port** — A virtual door on a computer that identifies 
which application or service incoming data belongs to. 
Every computer has 65,535 ports.

## Analogy
Think of your IP as a building and ports as individual doors:
- 192.168.1.5 : 80 → goes to web server
- 192.168.1.5 : 22 → goes to SSH service
- 192.168.1.5 : 443 → goes to HTTPS service

## Port Ranges
- Well-known ports: 0–1023
- Registered ports: 1024–49,151
- Dynamic ports: 49,152–65,535

## Key Ports Every Pentester Must Memorize

| Port | Protocol | Attack Relevance |
|------|----------|-----------------|
| 21 | FTP | Anonymous login, cleartext creds |
| 22 | SSH | Brute force, key attacks |
| 23 | Telnet | Cleartext — sniff credentials |
| 25 | SMTP | Email relay attacks |
| 53 | DNS | Zone transfer, DNS poisoning |
| 80 | HTTP | Web attacks, OWASP Top 10 |
| 443 | HTTPS | Same as HTTP, encrypted |
| 445 | SMB | EternalBlue, Pass-the-Hash |
| 3306 | MySQL | SQL injection, direct DB access |
| 3389 | RDP | Brute force, BlueKeep |

## Command
```bash
netstat -tulnp
# Shows all open ports on your machine right now

