# CH-08: Redirection, Pipes & Wildcards

## Overview
These symbols control how command output flows — sending it into files, chaining it into other commands, or matching multiple files at once.

## (i) Redirection
| Symbol | Definition |
|--------|------------|
| `>` | Sends output to a file, overwriting it completely |
| `>>` | Sends output to a file, appending to the end instead of overwriting |
| `<` | Feeds a file's contents into a command as input |

**Example:**
```bash
echo "hello" > file.txt   # file.txt now contains "hello"
echo "again" >> file.txt  # "again" is added as a new line, "hello" stays
sort < file.txt            # sort reads its input from file.txt
```

## (ii) Pipes
**`|`** → Takes the output of one command and feeds it directly as input into the next command, letting you chain tools together.

**Example:**
```bash
ps aux | grep ssh
```
→ `ps aux` lists all processes, and instead of printing everything, that output is piped into `grep ssh`, which filters it down.

## (iii) Wildcards
| Symbol | Definition |
|--------|------------|
| `*` | Matches any number of characters (including zero) |
| `?` | Matches exactly one character |

**Example:**
```bash
ls *.txt        # lists every file ending in .txt
ls file?.txt    # matches file1.txt, file2.txt, etc.
rm *.tmp        # deletes every file ending in .tmp
```

## Practical Example
```bash
ls -la > filelist.txt
cat /etc/passwd | grep bash
ls *.log
```

## Summary
- `>` overwrites a file with output, `>>` appends to it, `<` feeds a file in as input.
- `|` chains commands together.
- `*` matches any number of characters, `?` matches exactly one — both used for pattern-matching filenames.
