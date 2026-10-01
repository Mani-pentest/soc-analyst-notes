# CH-16: Log Files & Log Analysis

## Overview

Linux records system and application activity in log files, primarily stored under `/var/log`. Reviewing these logs reveals what has happened on a system — logins, errors, service activity, and sometimes accidentally exposed sensitive data.

**Note:** checking log permissions and contents is a standard enumeration step, since logs can reveal usernames, failed login attempts, and even leaked credentials.

## Key Log Files

| File | Contents |
|---|---|
| `/var/log/auth.log` (Debian/Kali) | Authentication attempts, sudo usage, SSH logins |
| `/var/log/syslog` | General system activity log |
| `/var/log/apache2/access.log` | Web server request history (if Apache is installed) |
| `/var/log/apache2/error.log` | Web server errors |

## Core Commands

| Command | Purpose |
|---|---|
| `cat /var/log/auth.log` | Prints the entire auth log |
| `tail -f /var/log/auth.log` | Follows the log live, showing new entries as they happen |
| `grep "Failed password" /var/log/auth.log` | Filters for failed SSH login attempts |
| `journalctl` | Views logs collected by systemd |
| `journalctl -u ssh` | Views logs for a specific service |
| `journalctl -f` | Follows the systemd journal live |

## Example

```bash
tail -f /var/log/auth.log
grep "Failed password" /var/log/auth.log
journalctl -u ssh
```

## Summary

- Most logs live under `/var/log`; `auth.log` and `syslog` are the two most commonly checked
- `tail -f` follows a log live; `grep` filters for specific events (like failed logins)
- `journalctl` is the modern tool for viewing systemd-managed logs, filterable by service with `-u`
