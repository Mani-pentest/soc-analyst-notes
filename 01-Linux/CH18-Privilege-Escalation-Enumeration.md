# CH-18: Privilege Escalation Enumeration

## Overview
Enumeration is the systematic process of gathering information about a system's permissions, SUID binaries, cron jobs, sudo rights, and writable files, in order to identify privilege escalation opportunities before attempting exploitation.

## Why Enumerate Manually First
Understanding what to check manually allows for correct interpretation of automated tool output. Manual enumeration matters for:
- Troubleshooting when automated tools fail
- Enumeration on restricted systems where uploading tools isn't possible

## Manual Enumeration Commands

| Command | Purpose |
|---|---|
| `whoami` / `id` | Confirms the current user and groups |
| `sudo -l` | Shows what commands the current user can run as another user via sudo |
| `uname -a` | Shows the kernel version, useful for checking known exploits |
| `cat /etc/os-release` | Shows OS details |
| `find / -perm -4000 -type f 2>/dev/null` | Lists SUID binaries |
| `find / -perm -2000 -type f 2>/dev/null` | Lists SGID binaries |
| `cat /etc/crontab` | Views system-wide cron jobs |
| `find / -writable -user root -type f 2>/dev/null` | Finds root-owned files writable by the current user |
| `ss -tulnp` | Shows active/listening network connections |

## Automated Enumeration: linPEAS
linPEAS is a downloadable script that automates manual privilege escalation checks — SUID/SGID, sudo misconfigurations, PATH issues, and more. It runs the same checklist above plus additional checks, color-coding results by likely exploitability.

| Command | Purpose |
|---|---|
| `./linpeas.sh` | Runs linPEAS if it's already downloaded and executable on the target |
| `curl -L <url> \| sh` | Downloads and executes linPEAS directly in memory, without saving to disk |

**Command breakdown of `curl -L <url> \| sh`:**
- `curl` — downloads a file from a URL
- `-L` — follows redirects
- `|` — pipes curl's output directly into the next command
- `sh` — executes the piped content as a shell script

## Summary
- Enumeration systematically maps permissions, SUID/SGID binaries, cron jobs, sudo rights, and writable files to spot privilege escalation opportunities
- Manual enumeration builds the understanding needed to troubleshoot tool failures and to work on restricted systems without upload access
- linPEAS automates the manual enumeration checklist and highlights likely exploitation paths by color
- `sudo -l` is one of the highest-value single commands to run .
