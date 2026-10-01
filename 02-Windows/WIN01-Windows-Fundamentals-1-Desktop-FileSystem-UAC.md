# WIN01: Windows Fundamentals 1 — Desktop, File System, System32, Users & UAC

## Windows Editions

| Edition | Key Feature |
|---|---|
| Windows Home | No BitLocker, no domain join |
| Windows Pro | Adds BitLocker encryption, Remote Desktop, domain join |
| Windows Enterprise | Volume licensing, advanced security controls |

**Note:** BitLocker is the key differentiator between Home and Pro editions.

## The Desktop (GUI)

| Component | Purpose |
|---|---|
| Taskbar | Pinned/running apps, system tray, search |
| Start Menu | Access to apps, settings, power controls |
| Desktop Icons | Shortcuts to files/programs |

## The File System

| File System | Notes |
|---|---|
| NTFS | Default Windows filesystem — supports permissions (ACLs), encryption, large file sizes |
| FAT32 | Legacy format, no permission support |

**Note:** NTFS permission support (ACLs) is what enables Windows access control — relevant later in privilege escalation.

## The Windows\System32 Folder

| Term | Meaning |
|---|---|
| `C:\Windows\System32` | Core system files, DLLs, and executables the OS depends on |
| `%WINDIR%` | System variable pointing to the Windows install directory |

**Note:** Attackers sometimes name malicious files to mimic legitimate System32 binaries — relevant for SOC endpoint investigation and pentest persistence detection alike.

## User Accounts, Profiles & Permissions

| Concept | Description |
|---|---|
| `lusrmgr.msc` | Opens Local Users and Groups management console |
| User Account | Individual login with its own profile and permission level |
| Group | Collection of users sharing a permission level (Users, Administrators, Guests) |
| Guest Account | Built-in limited account for temporary access |

**Note:** This local model is replaced by centralized management in Active Directory — covered next chapter.

## User Account Control (UAC)

UAC prompts for confirmation before allowing system changes that require admin rights, even when logged in as an administrator.

**Note:** UAC is a safety prompt, not a hard security boundary — UAC bypass is a recognized privilege escalation technique, relevant to your pentest track; UAC prompt logs are also a detection signal on the SOC side.
