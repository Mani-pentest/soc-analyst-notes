# WIN02: Windows Fundamentals 2 — System Config, UAC Settings, Resource Monitor, Registry

## System Configuration (MSConfig)
Used for advanced troubleshooting, mainly diagnosing startup issues. Access via `msconfig` in Run.

| Tab | Purpose |
|---|---|
| General | Startup mode options |
| Boot | Boot options, safe mode settings |
| Services | Enable/disable startup services |
| Startup | Manage startup programs |
| Tools | Quick launch shortcuts to other admin tools |

## UAC Settings
Command to open: `UserAccountControlSettings.exe`
UAC sensitivity can be adjusted on a slider (Never notify → Always notify) or disabled entirely (not recommended).

## Computer Management (compmgmt.msc)
Central hub combining three sections:

| Section | Contains |
|---|---|
| System Tools | Task Scheduler, Event Viewer, Device Manager |
| Storage | Disk Management |
| Services and Applications | Local Services, WMI Control |

**Note:** Task Scheduler manages automated tasks (run at login, logoff, or set intervals) — relevant later, since scheduled tasks are a common persistence/privesc mechanism. Event Viewer logs system events — an audit trail used to diagnose issues and investigate activity, directly relevant to SOC log analysis.

## System Information
Command: `msinfo32`
Gives a full snapshot of the machine — hardware specs, OS details, environment variables, installed software.

**Note:** The Environment Variables section here exposes system paths and configs — often referenced during command execution and enumeration.

## Resource Monitor
Command: `resmon.exe`
Geared toward advanced troubleshooting — shows real-time CPU, memory, disk, and network usage per process.

## Command Prompt Basics

| Command | Purpose |
|---|---|
| `hostname` | Displays computer name |
| `whoami` | Displays logged-in user |
| `ipconfig` | Shows network address settings |
| `netstat` | Shows active network connections |
| `<command> /?` | Opens help manual for that command |

## Registry Editor
Command: `regedit`
Central hierarchical database storing low-level OS and application settings.

**Note:** The Registry is a frequent target for persistence mechanisms in real attacks (e.g., Run keys) — understanding its structure now supports both detection (SOC) and exploitation awareness (pentest) later.
