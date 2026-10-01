# CH-14: Archiving & Compression

## Overview

Archiving combines multiple files/folders into a single file for easier storage or transfer. Compression reduces that file's size. On Linux, these are often done together using `tar` and `gzip`.

**Note:** you'll frequently need to package loot into a single file for transfer, or extract tools/wordlists distributed as compressed archives.

## Core Tool: tar (Tape Archive)

| Command | Purpose |
|---|---|
| `tar -cvf archive.tar folder/` | Creates an uncompressed archive from a folder |
| `tar -xvf archive.tar` | Extracts an uncompressed `.tar` archive |
| `tar -czvf archive.tar.gz folder/` | Creates a compressed archive (tar + gzip) |
| `tar -xzvf archive.tar.gz` | Extracts a compressed `.tar.gz` archive |

**Flag breakdown:**

| Flag | Meaning |
|---|---|
| `c` | create a new archive |
| `x` | extract an archive |
| `z` | compress/decompress using gzip |
| `v` | verbose — show files as they're processed |
| `f` | specify the filename immediately after |

## Single-File & Zip Tools

| Command | Purpose |
|---|---|
| `gzip file.txt` | Compresses a single file, replacing it with `file.txt.gz` |
| `gunzip file.txt.gz` | Decompresses a `.gz` file back to its original |
| `zip archive.zip file1 file2` | Creates a `.zip` archive from specified files |
| `unzip archive.zip` | Extracts a `.zip` archive |

## Examples

```bash
tar -cvf backup.tar /home/kali/loot
tar -czvf backup.tar.gz /home/kali/loot
tar -xzvf backup.tar.gz -C /tmp/
```

## Summary

- `tar` bundles files together; adding `z` also compresses them with gzip
- `c` = create, `x` = extract, `z` = compress/decompress, `v` = verbose, `f` = filename
- `gzip`/`gunzip` handle single-file compression; `zip`/`unzip` handle the more universal `.zip` format
