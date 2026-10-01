# CH-03: Users, Groups & Privilege Model

## Overview
The privilege model keeps track of every process and controls how much power each user/process has.

## Three Categories
- **root** — Superuser, unrestricted access to the entire system
- **Regular user** — Normal account with limited privileges
- **Service user** — Account created for running a specific program (no login shell)

## Identity Commands
| Command | Purpose |
|---------|---------|
| `id` | Displays current UID/GID and group memberships<br>e.g. `uid=1000(kali) gid=1000(kali) groups=1000(kali)` |
| `whoami` | Prints the current logged-in username |
| `w` | Shows who's logged in and what they're doing |

## Privilege Escalation Commands
- **`sudo <command>`** — Allows a permitted user to run a single command with root privileges. Authenticated using **your own** password.
- **`su`** — Switches the active session entirely to a different user (commonly root). Authenticated using the **target user's** password.

| | Scope | Password used |
|---|---|---|
| `sudo` | One command, then back to normal user | Your own |
| `su` | Switches the whole session | Target user's |

## Key Files
### `/etc/passwd`
Stores info about every user account on the system (readable by everyone).

username:x:UID:GID:home_directory:shell

Example: `kali:x:1000:1000::/home/kali:/bin/bash`

### `/etc/shadow`
Stores the actual **encrypted** passwords for each user. Readable only by root.

## Useful Commands
```bash
cat /etc/passwd                    # list all user accounts
cat /etc/passwd | grep <username>  # show one user's config
```

## Summary
- `sudo` grants temporary root access for a single command.
- `su` switches your entire session to another user.
- Account info lives in `/etc/passwd`; actual (encrypted) passwords live in `/etc/shadow`.
