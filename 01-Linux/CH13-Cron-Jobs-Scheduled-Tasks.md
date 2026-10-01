# CH-13: Cron Jobs & Scheduled Tasks

## Definition
Cron is a Linux job scheduler — it lets you set up commands (or scripts) to run automatically at specific times or intervals, without you manually triggering them.

## Why It Matters
For pentesting: a cron job scheduled to run and owned by root that points to a file writable by a regular user is a common privilege escalation vector.

## Commands
| Command | Definition |
|---------|------------|
| `crontab -l` | List the scheduled jobs for the current user |
| `crontab -e` | Open the current user's crontab for editing |
| `sudo crontab -l -u root` | List root's scheduled jobs |
| `cat /etc/crontab` | View the system-wide scheduled jobs |
| `ls -la /etc/cron.d/` | List additional cron job definitions |

## Cron Syntax
`* * * * * command-to-run`

| Position | Meaning | Range |
|----------|---------|-------|
| 1st | Minute | 0–59 |
| 2nd | Hour | 0–23 |
| 3rd | Day of month | 1–31 |
| 4th | Month | 1–12 |
| 5th | Day of week | 0–6 (Sunday=0) |

`*` in any position means "every" — for that unit.

## Entry Examples

| Cron Entry | Meaning |
|------------|---------|
| `0 5 * * * /home/kali/backup.sh` | Run `backup.sh` daily at 5:00 AM |
| `*/10 * * * * /home/kali/check.sh` | Run `check.sh` every 10 minutes |
| `0 0 * * 0 /home/kali/weekly-report.sh` | Run `weekly-report.sh` every Sunday at midnight |

## Privilege Escalation Concept
If `/etc/crontab` contains an entry like:
`* * * * * ` root /opt/scripts/cleanup.sh
— check whether that script is writable by the current user. If `/opt/scripts/cleanup.sh` is writable by your user.

**Command:**
```bash
ls -l /opt/scripts/cleanup.sh
```
- If it is, modifying its contents allows arbitrary commands to run as root the next time cron.

## Summary

- Cron automates command execution on a schedule, defined by 5 time fields.
- Crontab -l/-e manage a user's own jobs; system-wide jobs live in /etc/crontab and /etc/cron.d/ .
- A root-scheduled script writable by a regular user is a privilege escalation opportunity.
