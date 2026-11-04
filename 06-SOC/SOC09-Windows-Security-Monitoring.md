# SOC09: Windows Security Monitoring

Covers how a SOC analyst detects common Windows attacks directly on the host, using Event Viewer, Sysmon, and PowerShell history without a SIEM. Four rooms: Windows Logging for SOC, Windows Threat Detection 1, Windows Threat Detection 2, Windows Threat Detection 3.

## The Two Log Sources

| Source | Answers | Location in Event Viewer |
|---|---|---|
| Security log | Who logged on, from where, and which accounts changed | Windows Logs > Security |
| Sysmon | What ran, what files were created, which connections and DNS queries were made, and which registry keys changed | Applications and Services Logs > Microsoft > Windows > Sysmon > Operational |

**Note:** Sysmon is not installed by default. It gives much richer detail than the Security log's process events, so a SOC usually deploys it on endpoints.

## Event ID Cheat Sheet

| Security Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4720 | User account created |
| 4732 | User added to a security group |
| 4724 | Password reset attempt |
| 4688 | Process created (basic, noisy) |
| 4697 | Service installed |
| 4698 | Scheduled task created |

| Sysmon Event ID | Meaning |
|---|---|
| 1 | Process creation (hashes, parent process, command line) |
| 3 | Network connection |
| 11 | File created |
| 13 | Registry value set |
| 15 | File stream created (marks a downloaded file) |
| 22 | DNS query |

| Logon Type | Meaning |
|---|---|
| 2 | Interactive (at the keyboard) |
| 3 | Network |
| 10 | RemoteInteractive (RDP) |

## Windows Logging for SOC

Event Viewer is the built-in log tool. Open it with Win + R and `eventvwr`. Use Filter Current Log to show one Event ID, and Action > Open Saved Log to read an exported `.evtx` file.

| Task | How |
|---|---|
| Find failed logons | Security log, filter Event ID 4625 |
| Find successful logons | Security log, filter Event ID 4624 |
| See which programs ran | Sysmon log, filter Event ID 1 |
| Find downloads and dropped files | Sysmon log, filter Event ID 11 |
| Find DNS lookups | Sysmon log, filter Event ID 22 |

PowerShell commands can be read from the history file:

```
C:\Users\<user>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```

Script block logging records the code PowerShell runs as Event ID 4104.

**Note:** Start from one suspicious event and search around its timestamp instead of scrolling the whole log. The Logon ID field connects a logon to everything that session did afterward.

## Windows Threat Detection 1

The room covers Initial Access: how attackers first get onto a Windows host. There are two groups: methods that need an exposed service, and methods that rely on a person.

| Method | MITRE | Detection |
|---|---|---|
| Exposed RDP, VNC, or SSH with weak passwords | T1133 External Remote Services | Security 4625 then 4624 |
| Vulnerable public-facing server (for example a mail server) | T1190 | Logs from the server, then Sysmon on the host |
| Phishing attachment | T1566 | Sysmon 1, 11, 22 |
| Infected USB drive | T1091 | Sysmon 1 with a non-C: drive in the Image path |

### RDP brute force

A botnet scans for an exposed RDP port and then sprays common passwords, most often against the built-in Administrator account.

| Step | What to look at |
|---|---|
| 1. Find the attack | Filter 4625 with Logon Types 3 and 10 and look for one source IP hitting an account many times |
| 2. Confirm the breach | Filter 4624 with Logon Type 10 from the same IP |
| 3. Identify the attacker machine | The Workstation Name field in the 4624 event shows the attacker's real hostname |
| 4. See what they did | Copy the Logon ID, search Sysmon for it, and read the Event ID 1 process creations |

### Phishing attachments

| Delivery | Key sign |
|---|---|
| A `.com` file disguised as a web address | Double extension and a hidden real extension |
| A shortcut (`.lnk`) that runs PowerShell | `explorer.exe` starts PowerShell directly; a file was created in Downloads first |
| A ZIP containing a double-extension file such as `.jpg.exe` | Browser downloads the ZIP, Explorer unpacks it, and the malware runs as a child of `explorer.exe` |

Sysmon chain for an archive-delivered attack:

```
Browser starts (Sysmon 1)
  → ZIP downloaded (Sysmon 11)
    → file unpacked by explorer.exe (Sysmon 11)
      → malware runs (Sysmon 1)
        → DNS query to the attacker domain (Sysmon 22)
```

### USB infection

Malware run from a removable drive shows a non-standard drive letter such as `E:\` in the Sysmon Event ID 1 Image field. It may copy itself to the system and spread to other drives.

**Note:** A file created in Downloads before execution is often the only sign that an attack started with a phishing attachment, so always check Sysmon 11 around the first execution.

## Windows Threat Detection 2

The room covers what attackers do right after they get in: Discovery, Collection, Exfiltration, and Ingress Tool Transfer.

### Discovery

Attackers run built-in commands to learn about the host, such as who they are, network settings, users, and running processes. Some also check whether an EDR product is installed.

Build a process tree with Sysmon Event ID 1 by matching `ProcessId` to `ParentProcessId`:

```
suspicious.exe
  → cmd.exe
      → whoami
      → ipconfig /all
      → net user
      → tasklist
```

**Note:** No single command proves an attack. The signal is a burst of short-lived reconnaissance processes spawned in quick succession by a non-system parent.

### Collection and exfiltration

| Type | How it looks in Sysmon |
|---|---|
| Manual collection | Notepad, WordPad, or PowerShell opened with sensitive files such as keys and documents as arguments |
| Automated stealer | No cmd or PowerShell. Creates a staging folder, copies targeted file types such as docx, pdf, and xlsx, and may capture clipboard contents |
| Exfiltration | Outbound connection (Sysmon 3) or DNS query (Sysmon 22) from the suspicious process, often to a trusted cloud storage service |

**Note:** A trusted destination such as cloud storage proves nothing by itself. Judge the process making the connection.

### Ingress tool transfer (T1105)

Attackers download extra tools after the first foothold, so the first payload stays small.

| Method | Detection |
|---|---|
| PowerShell Invoke-WebRequest | Sysmon 1: `powershell.exe` with download arguments |
| `curl.exe` | Sysmon 1 for `curl.exe`, then Sysmon 3 for the connection |
| `certutil.exe` with `-urlcache` | Sysmon 1: a legitimate tool used for download |
| Browser or RDP clipboard | Sysmon 11: a file appears in Downloads with no browser involved |

## Windows Threat Detection 3

The room covers how attackers keep access to a compromised host: command and control (C2) and persistence.

### Command and control (T1071)

A phishing attachment may download a second C2 binary and hide it in a user folder such as AppData, so the attack survives if the original file is deleted. The C2 domain often imitates a legitimate service name.

**Note:** Process context matters more than the domain name. A program in AppData contacting a lookalike domain is far more suspicious than a browser visiting the same site.

### Persistence

| Method | MITRE | Detection |
|---|---|---|
| Backdoor user account | T1136 / T1098 | Security 4720 (created), 4732 (added to a group), linked by Logon ID |
| Windows service | T1543.003 | Security 4697, Sysmon 1 for `sc.exe` with `create`. Runs at startup as SYSTEM |
| Scheduled task | T1053.005 | Security 4698, Sysmon 1 for `schtasks.exe` with `/create` |
| Startup folder | T1547.001 | Sysmon 11 for a new file in the user's Startup folder |
| Run registry key | T1547.001 | Sysmon 13 for a write to `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` |

The Startup folder is under `C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\`.

**Note:** Programs launched from the Startup folder have `explorer.exe` as their parent, exactly like programs the user starts by hand. The file creation event in the Startup path is the reliable signal. Tasks that run later show `svchost.exe -s Schedule` as the parent.

## Correlation Model

| Shared Field | Links These Events |
|---|---|
| Logon ID | 4624 logon, then 4720 account creation, then 4732 group add |
| ProcessId and ParentProcessId | Sysmon 1 process chain from malware to the commands it runs |
| ProcessId | Sysmon 1 execution, then 3 network, 11 file drop, and 22 DNS from the same process |
| Drive letter in Image path | Reveals execution from a USB drive |

## MITRE ATT&CK Coverage

| Tactic | Technique | Detection |
|---|---|---|
| Initial Access | T1133 External Remote Services (RDP) | Security 4625 and 4624 (Type 10) |
| Initial Access | T1566 Phishing | Sysmon 1 and 11 process tree |
| Initial Access | T1091 Removable Media | Sysmon 1 with a non-C: Image path |
| Discovery | T1033, T1082, T1057 | Sysmon 1 burst of child processes |
| Collection | T1005 Local Data | Sysmon 1 file access commands |
| Exfiltration | T1041 | Sysmon 3 and 22 from a suspicious process |
| Command and Control | T1071 | Sysmon 3 and 22 from an AppData process |
| Ingress Tool Transfer | T1105 | Sysmon 1 for certutil, curl, or PowerShell, then Sysmon 3 |
| Persistence | T1136, T1098 | Security 4720, 4732, 4724 |
| Persistence | T1543.003, T1053.005, T1547.001 | Security 4697, 4698, Sysmon 11 and 13 |

## Investigation Workflow

| Step | Action |
|---|---|
| Anchor | Find the first suspicious event and note its time |
| Who | Check the Security log: logon type, source, account, Logon ID |
| What | Check Sysmon 1: image, command line, parent process |
| Trace | Follow the ProcessId to Sysmon 11 (files), 3 (network), 22 (DNS), and 13 (registry) |
| Persistence | Look for new users, services, scheduled tasks, Startup files, and Run keys |
| Report | Write the timeline with evidence and recommended actions |

## Common Mistakes

- Judging a process by its name alone instead of its parent, path, and command line
- Treating a trusted cloud destination as safe without checking the process that connected
- Missing the Startup folder file creation because the process looks like a normal explorer.exe child
- Relying on Event ID 4688 when Sysmon 1 has more detail
- Stopping at the successful logon instead of following the Logon ID to what the session did

## Module Takeaway

Windows Logging for SOC teaches the two log sources and the key Event IDs. The three Threat Detection rooms follow an attack in order: Initial Access through RDP, phishing, and USB; then Discovery, Collection, Exfiltration, and tool transfer; then C2 and five persistence methods. The two anchors that tie everything together are the Logon ID for sessions and the ProcessId for what a program did. The earlier you catch an attack in this chain, the cheaper it is to contain.
