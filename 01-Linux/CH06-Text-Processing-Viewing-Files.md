# CH-06: Text Processing & Viewing Files

## Overview
These commands let you view, scan, and filter the contents of text files without opening a full editor — essential for reading logs, config files, and command output quickly.

## (i) Viewing Files
| Command | Definition |
|---------|------------|
| `cat <file>` | Prints the entire file contents to the screen at once |
| `less <file>` | Opens the file page-by-page |
| `head <file>` | Shows the first 10 lines of a file |
| `tail <file>` | Shows the last 10 lines of a file |
| `tail -f <file>` | "Follows" a file live — keeps showing new lines as they're added (great for watching logs in real time) |

## (ii) Searching Inside Files
**`grep`** — Searches text for lines matching a pattern.

**Example:**
```bash
grep "root" /etc/passwd
grep -i "error" logfile.txt
grep -r "password" /etc/
```
- `-i` → case insensitive
- `-r` → search recursively through a directory

## Intro to sed and awk
**`sed`** → Stream editor, mainly used to find-and-replace text.

**Example:** `sed 's/old/new/' file.txt`
→ Replaces the first `old` with `new` on each line and prints the result.

**`awk`** → Used to process text column by column.

**Example:** `awk '{print $1}' file.txt`
→ Prints just the first column of every line.

## Practical Example
```bash
cat /etc/hostname
head -n 5 /etc/passwd
grep "bash" /etc/passwd
```

## Summary
- `cat` dumps a whole file; `less` scrolls through it; `head`/`tail` shows the start/end.
- `tail -f` is essential for watching logs live.
- `grep` searches for matching text; `-i` ignores case, `-r` searches recursively.
- `sed` finds/replaces text; `awk` processes text by column.
