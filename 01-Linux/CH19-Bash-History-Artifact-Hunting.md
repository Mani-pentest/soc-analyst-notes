# CH-19: Bash History & Artifact Hunting

## Overview
Bash history and artifact hunting is the process of locating traces on a system — command history, temporary files, credentials, SSH keys — that reveal sensitive information on prior activity.

## Bash History

| Command | Purpose |
|---|---|
| `cat ~/.bash_history` | Shows the current user's full saved command history |
| `history` | Shows the current shell session's command history |
| `cat ~/.bash_history \| grep -i "pass"` | Filters history for keyword matches like passwords |
| `find / -name "*.bash_history" 2>/dev/null` | Finds history files for other users |

## Other Artifact Locations

| Command | Purpose |
|---|---|
| `ls -la /home/*/` | Reveals hidden config files and keys in user home directories |
| `find / -name "id_rsa" 2>/dev/null` | Finds SSH private keys |
| `find / -name "*.pem" 2>/dev/null` | Finds certificate/key files |
| `ls -la /tmp/ ; ls -la /var/tmp/` | Checks temp directories for leftover tools or old output |
| `grep -ri "password" /var/log/*.log 2>/dev/null` | Searches logs for accidentally logged credentials |
| `find / -mmin -60 -type f 2>/dev/null` | Finds files modified in the last 60 minutes |

## Practical Example
```bash
cat ~/.bash_history | grep -i "pass"
find / -name "id_rsa" 2>/dev/null
ls -la /tmp/
grep -ri "password" /var/log/*.log 2>/dev/null
```

## Summary
- `.bash_history` frequently contains credentials typed directly into commands
- `/tmp` and `/var/tmp` commonly hold leftover tools or forgotten output
- `find -mmin` locates recently modified files, useful for spotting recent activity
