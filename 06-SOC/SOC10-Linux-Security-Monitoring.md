# SOC10: Linux Security Monitoring

Covers how a SOC analyst reads Linux logs and detects common Linux attacks directly on the host, using text logs, the systemd journal, and auditd. Four rooms: Linux Logging for SOC, Linux Threat Detection 1, Linux Threat Detection 2, Linux Threat Detection 3.

## The Three Log Sources

| Source | What It Gives | Tool |
|---|---|---|
| Text logs in `/var/log` | High-level events: logins, sudo use, package installs, web requests | `cat`, `grep`, `head` |
| systemd journal | Service logs, such as the SSH service | `journalctl` |
| auditd | Syscall-level records: which program ran, which file was touched, with the full process tree | `ausearch` |

**Note:** Linux logs are mostly plain text and less structured than Windows logs. Start with syslog and auth.log for visibility, then use auditd as the ground truth when you need to prove exactly what ran.

## Log File Cheat Sheet

| File | Contains | Look For |
|---|---|---|
| `/var/log/auth.log` (`/var/log/secure` on Red Hat family) | Logins, SSH, sudo, user management | Failed passwords, new users, sudo group changes |
| `/var/log/syslog` | General system messages, time sync, kernel messages | Unexpected services and kernel messages |
| `/var/log/dpkg.log` | Package installs and versions | Unexpected tools installed |
| `/var/log/nginx/access.log` | Web requests | Commands in URL parameters |
| `~/.bash_history` | Commands a user typed | Attacker commands left behind |
| `/var/log/audit/audit.log` | auditd records | Process, file, and network activity |

## Command Cheat Sheet

| Command | Purpose |
|---|---|
| `grep "sshd" /var/log/auth.log` | All SSH events |
| `grep "Failed" /var/log/auth.log` | Failed logins |
| `grep "Accepted" /var/log/auth.log` | Successful SSH logins, showing password or public key |
| `grep -E "useradd\|usermod" /var/log/auth.log` | User creation and group changes |
| `grep "sudo" /var/log/auth.log` | Sudo activity |
| `journalctl -u ssh.service -o short-iso` | SSH service log with ISO timestamps |
| `ausearch -i -if audit.log` | Read an exported auditd log with names decoded |
| `ausearch -i -x crontab` | Find events by executable |
| `ausearch -i -f /.ssh/authorized_keys` | Find events by file |
| `ausearch -k <key>` | Find events by an audit rule key |

## Windows and Linux Side by Side

| Question | Windows | Linux |
|---|---|---|
| Failed logon | Security 4625 | auth.log "Failed password" |
| Successful logon | Security 4624 | auth.log "Accepted password" or "Accepted publickey" |
| User created | Security 4720 | auth.log `useradd` |
| Added to admin group | Security 4732 | auth.log `usermod` to the sudo group |
| Process started | Sysmon 1 | auditd `execve` records, searched with `ausearch -x` |
| Scheduled persistence | Scheduled task | cron job |
| Service persistence | New service | systemd service file |

## Linux Logging for SOC

| Task | Log and Method |
|---|---|
| Time sync and kernel messages | `/var/log/syslog`, filter with `grep` |
| Failed SSH logins across several users | `auth.log`, filter for `Failed` and `sshd` |
| User created and added to the sudo group | `auth.log`, filter for `useradd` and `usermod` |
| What was installed and which version | `/var/log/dpkg.log` |
| What a user ran | `.bash_history` in the home folder |
| File access, program execution, downloads, scanning | auditd, searched with `ausearch` |

`execve` is the Linux system call that runs a program, so process execution appears in auditd as `execve` records. auditd rules can also tag events with a key (for example a key for a specific file or for `wget`), which makes them easy to find later.

**Note:** Bash history shows what a user typed, but an attacker can clear it. Auditd records are much harder to hide because they come from the kernel.

## Linux Threat Detection 1

The room covers Initial Access on Linux through SSH and through exposed web services.

### SSH

SSH is the most common way attackers get a first foothold on a Linux server.

| Attack | Sign in auth.log |
|---|---|
| Password brute force | Many `Failed password` entries from one IP, often across several usernames |
| Brute force that worked | A `Failed password` burst, then `Accepted password` from the same IP |
| Stolen SSH key | `Accepted publickey` with no failed attempts before it |

Questions to ask for any successful SSH login: was it a password or a key, did brute force come before it, and is this the first login for that account from that IP. SSH keys can be stolen from places such as code repositories and automation servers, or from an admin laptop infected with a data stealer.

**Note:** A key login with no failed attempts is not safe by default. Stolen keys leave no brute-force trail, so check whether the key owner really logged in.

### Exposed web services

Any public application can be an entry point. In the room's scenario, a web app passed user input to the shell, which is command injection.

| Sign in `/var/log/nginx/access.log` | Meaning |
|---|---|
| Shell commands inside URL parameters, such as `;whoami` or `;ls` | Command injection |
| Requests for files the app should not serve | Probing or file theft |

### Process tree analysis

auditd shows which process started which. A web application normally does not launch shells.

```
python web app (parent)
  → /bin/sh -c <command>
      → attacker commands
```

When a trusted application suddenly runs unexpected commands, suspect exploitation of that application (T1190). Process tree analysis with auditd is the method that works across many Initial Access techniques.

## Linux Threat Detection 2

The room covers what attackers do after getting in: Discovery, tool delivery, and a real cryptominer campaign.

| Stage | What Happens | How to See It |
|---|---|---|
| Discovery | Commands that identify the OS, hostname, environment, processes, and whether the host is a virtual machine or sandbox | `ausearch -i -x <command>` for each command |
| Delivery | Tools copied in with `scp`, `wget`, or `curl` | auditd execve records for those programs |
| Persistence | The attacker makes sure they can return | Covered in Linux Threat Detection 3 |
| Impact | A cryptominer runs on the server (resource hijacking, T1496) | High CPU and unknown processes |

The campaign in the room started with SSH brute force, then discovery, then tool transfer, then the miner.

**Note:** Context decides whether discovery is malicious. A troubleshooting script run by IT can run the same commands as an attacker. Always find the parent process or script, who started it, and why.

## Linux Threat Detection 3

The room covers the later stages of Linux intrusions: reverse shells, privilege escalation, and persistence.

### Reverse shells

An attacker abuses a flaw such as command injection to make the server connect out to a system they control.

- The command-injection request shows in the web log.
- auditd shows the web app's child process starting a network tool such as `socat`, with the attacker's address in the arguments.
- Search the exported audit log, decode with `-i`, and look at the process title (`proctitle`) field.

### Privilege escalation

After landing as a low-privilege user, attackers search for credentials in files and look for ways to become root. auditd shows the searching commands (for example `find` and `grep` looking for the word "pass") with their arguments.

### Persistence

| Method | Where It Lives | How to Detect |
|---|---|---|
| Cron job | `/var/spool/cron/crontabs/<user>` | `ausearch -i -x crontab`, then look in the spool folder |
| systemd service | `/etc/systemd/system/` | `ausearch` for file creation in that path |
| New user account | `/etc/passwd` and auth.log | `grep -E "useradd\|usermod" /var/log/auth.log` |
| SSH key backdoor | `~/.ssh/authorized_keys` (including `/root/.ssh/`) | `ausearch -i -f /.ssh/authorized_keys` |

The room describes five common persistence techniques. The four above are the ones covered with hands-on detection steps.

## Correlation Model

| Shared Field | Links These Events |
|---|---|
| Source IP | Failed password burst, then accepted login, then later activity |
| Parent and child process | Web app, then shell, then attacker command |
| Timestamp | Login time, then first discovery command, then tool download |
| File path | A new cron entry, service file, or authorized_keys change tied to the session that made it |

## MITRE ATT&CK Coverage

| Tactic | Technique | Detection |
|---|---|---|
| Initial Access | T1110 Brute Force (SSH) | auth.log failed then accepted |
| Initial Access | T1078 Valid Accounts (stolen SSH key) | auth.log `Accepted publickey` without failures |
| Initial Access | T1190 Exploit Public-Facing Application | Web log command injection, auditd process tree |
| Execution | T1059.004 Unix Shell | auditd execve of `sh -c` |
| Discovery | T1082, T1033, T1057 | `ausearch -x` for discovery commands |
| Command and Control | T1105 Ingress Tool Transfer | scp, wget, or curl in auditd |
| Impact | T1496 Resource Hijacking | Unknown high-CPU process (cryptominer) |
| Persistence | T1053.003 Cron | Crontab spool changes |
| Persistence | T1543.002 Systemd Service | New file in `/etc/systemd/system/` |
| Persistence | T1136.001 Create Local Account | auth.log `useradd` |
| Persistence | T1098.004 SSH Authorized Keys | Changes to `authorized_keys` |

## Investigation Workflow

| Step | Action |
|---|---|
| Triage | Start with auth.log, syslog, and web logs for the first suspicious event |
| Entry | Decide how the attacker got in: SSH password, SSH key, or an exposed web service |
| Trace | Use auditd to build the process tree and read exact commands |
| Escalation | Look for credential searches and sudo or root activity |
| Persistence | Check cron, systemd, new users, and `authorized_keys` |
| Report | Write the timeline with evidence and recommended actions |

## Common Mistakes

- Treating a successful SSH login as safe because there were no failed attempts, when the login may have used a stolen key
- Judging discovery commands without finding out which script or user ran them
- Relying on `.bash_history` when auditd has records the attacker cannot easily remove
- Searching for the wrong case: Linux names and paths are case sensitive, so a `grep` for lowercase can miss a match
- Checking only cron and forgetting systemd services and `authorized_keys`

## Module Takeaway

Linux Logging for SOC teaches the three trails: text logs, the journal, and auditd. Linux Threat Detection 1 follows how attackers get in, through SSH brute force, stolen keys, and exploited web applications, and teaches process tree analysis. Linux Threat Detection 2 covers discovery, tool delivery, and a cryptominer campaign, with context deciding what is malicious. Linux Threat Detection 3 covers reverse shells, privilege escalation, and four persistence methods. The same story you learned on Windows applies here with different log names: brute force, login, look around, fetch tools, escalate, and stay.
