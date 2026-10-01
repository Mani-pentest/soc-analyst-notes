# CH-07: File Searching

## Overview
Commands used to locate files and programs on the filesystem, either by name, type, or other attributes.

## Commands

### (i) find
The most powerful and flexible search tool; searches the actual filesystem in real time.

**Example 1:**
```bash
find / -name "config.php" 2>/dev/null
```
→ Searches everywhere (`/`) for a file named exactly `config.php`.

**Example 2:**
```bash
find / -iname "config.php" 2>/dev/null
```
→ `-iname` = case-insensitive name search.

**Example 3:**
```bash
find / -type d -name "backup*" 2>/dev/null
```
→ `-type d` restricts results to directories only; `backup*` matches anything starting with "backup".

### (ii) locate
Much faster than `find`, but searches a pre-built index instead of the live system.

**Example:** `locate passwd`
→ Faster, but the index needs occasional refreshing with `sudo updatedb`, or it may miss recently created files.

### (iii) which
Shows the full path of a command, based on your `$PATH`.

**Example:**
```bash
which nmap
# /usr/bin/nmap
```
→ Useful to confirm a tool is installed and see exactly which version/location is being executed.

### (iv) whereis
Similar to `which`, but also shows related files like man pages and sources.

**Example:**
```bash
whereis nmap
# nmap: /usr/bin/nmap /usr/share/man/man1/nmap.1.gz
```

## Practical Example
```bash
which python3
find /etc -name "*.conf" 2>/dev/null
locate shadow
```

## Summary
- `find` searches the live filesystem — slower but always accurate and highly flexible with flags.
- `locate` searches a pre-built index — much faster, can be outdated.
- `which` shows where a command is located; `whereis` adds man pages and source files too.
