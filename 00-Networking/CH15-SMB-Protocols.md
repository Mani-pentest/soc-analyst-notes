# CH-15: Key Protocols in Networking

## SMB (Server Message Block)
**Definition** — A Windows network protocol used for file 
sharing, printer sharing and communication. Port 445. 
Major attack target in every internal pentest.

**EternalBlue (MS17-010)** — A critical SMB vulnerability 
exploited by NSA and leaked publicly. Used in WannaCry 
ransomware. Allows remote code execution without credentials.

**Share Enumeration** — Listing all available SMB shares 
on a target to find accessible files and folders.

### SMB Commands
```bash
smbclient -L //192.168.1.10         # List all shares
smbclient //192.168.1.10/sharename  # Connect to share
enum4linux 192.168.1.10             # Full SMB enumeration

FTP Anonymous Login

A misconfiguration where FTP server allows login
without a password. First thing to check when port 21 open.
ftp 192.168.1.10
# Username: anonymous
# Password: (press enter — leave blank)

SNMP (Simple Network Management Protocol)

Port 161 UDP. Used to monitor network devices.
Often has default community strings: “public” and “private”
snmpwalk -c public -v1 192.168.1.1
snmpenum -t 192.168.1.1


