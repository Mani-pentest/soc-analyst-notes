# CH-04: Package Management

## Overview
Package management is the system Linux uses to install, update, and remove software through a central tool — instead of manually downloading and running installers like on Windows.

**Note:** `update` and `upgrade` are two separate steps that work together, not one action.

## Core Tool: APT (Advanced Package Tool)
Used on Debian/Ubuntu-based systems (Kali is Debian-based).

| Command | Purpose |
|---------|---------|
| `sudo apt update` | Refreshes the local list of available packages from repositories (doesn't install/upgrade anything itself) |
| `sudo apt upgrade` | Installs newer versions of already-installed packages |
| `sudo apt install <package>` | Downloads and installs a new package |
| `sudo apt remove <package>` | Uninstalls a package but leaves config files behind |
| `apt search <term>` | Searches for a package by name when the exact name isn't known |

## Low-Level Tool: dpkg (Debian Package)
| Command | Purpose |
|---------|---------|
| `dpkg -i <package>.deb` | Installs a manually downloaded `.deb` file directly |
| `dpkg -l` | Lists all currently installed packages |
| `dpkg -l \| grep <name>` | Filters the installed package list for a specific package |
| `sudo apt --fix-broken install` | Fixes broken installs — run immediately after a failed `dpkg -i` |

## Where to Use Each
- **apt** → installing software from official repositories
- **dpkg** → installing a manually downloaded `.deb` file
