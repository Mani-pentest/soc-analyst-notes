# CH-01: Linux Filesystem & Navigation

## Overview
Linux uses a single, unified filesystem tree rooted at `/`. Unlike Windows (which has separate drive letters like C:, D:), every disk or partition gets mounted somewhere into this one tree.

## Key Directories
| Directory | Purpose |
|-----------|---------|
| `/` | The root of the entire filesystem — everything branches from here |
| `/home/<user>` | Personal directory for a user account |
| `/etc` | System-wide configuration files, typically world-readable |
| `/tmp` | Temporary storage for files |
| `/var/log` | System and application log files |
| `/root` | Home directory of the root account |

## Absolute vs Relative Path
- **Absolute Path** — A path starting from `/`. Example: `cd /etc`
- **Relative Path** — A path based on your current location. Example: `cd Documents`

## Navigation Commands
| Command | Purpose |
|---------|---------|
| `ls` | List the contents of a directory |
| `ls -a` | List all, including hidden files/dirs (starting with `.`) |
| `ls -l` | Long format — shows permissions, owner, size, modification time |
| `ls -la` | Combines both: hidden files in long format |
| `cd` | Change directory — move into a new location |
| `cd ..` | Move up one level |
| `cd ~` / `cd` | Go to home directory |
| `cd -` | Return to previous directory |
| `pwd` | Print working directory — shows full path of current location |

## Summary
- Linux has one root-based filesystem tree.
- `/home`, `/etc`, `/tmp`, `/var/log`, `/root` are the most security-relevant locations to know.
- `pwd` tells you where you are, `ls` shows you what's there, `cd` moves you between directories.
