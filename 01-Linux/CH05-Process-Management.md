# CH-05: Process Management

## Overview
A process is a running instance of a program, given a unique identifier called a **PID** (Process ID).

Process management covers viewing what's currently running, monitoring resource usage, stopping processes, and running tasks in the background.

## Why It Matters
You constantly need to check what's running on a target system and kill processes that interfere with your work.

## (i) Viewing Processes
| Command | Definition |
|---------|------------|
| `ps` | Shows processes running in the current terminal session only |
| `ps aux` | Shows every process on the system from every user, with CPU usage |
| `ps aux \| grep <term>` | Filters the full process list down to matches for a keyword |

**Example:** `ps aux | grep ssh`

## (ii) Live Monitoring
| Command | Definition |
|---------|------------|
| `top` | Live, auto-updating view of processes, sorted by CPU usage. Press `q` to quit |
| `htop` | Color-coded, more interactive version of `top` |

## (iii) Stopping Processes
Every running process has a unique number (PID). It will appear once you run `ps aux` or `top` — showing the specific process you want to stop.

| Command | Definition |
|---------|------------|
| `kill <PID>` | Sends a graceful shutdown request to a process by its PID |
| `kill -9 <PID>` | Forces an immediate, hard stop |
| `killall <name>` | Kills all processes matching a program name, without needing exact PIDs |

**Example:** `kill 1234`, `killall firefox`, `kill -9 1234`

## (iv) Running in the Background
| Command | Definition |
|---------|------------|
| `<command> &` | Runs a command in the background |
| `jobs` | Lists everything currently running in the background |
| `nohup <command> &` | Keeps a background process running even after the terminal closes |

**Example:**
```bash
nmap -sV 10.10.10.104
nohup nmap -sV 10.10.10.104
```
