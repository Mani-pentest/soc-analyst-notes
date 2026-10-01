# WIN04: Windows Privileges — User Rights Assignment

User Rights Assignment governs the methods by which a user can log on to a system and what actions they can perform once logged on. Rights are applied at the local device level, or centrally through domain Group Policy, and are managed via `secpol.msc` (Local Security Policy) or `gpedit.msc` (Local Group Policy Editor).

**Note:** Local Security Policy is only available on Windows Pro, Enterprise, and Education editions — not Home.

## Location
`Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment`

## Two Categories of User Rights

| Type | Description |
|---|---|
| Logon Rights | Control who is authorized to log on to a device and how (locally, via RDP, over the network) |
| Permissions | Control access to computer and domain resources — can override permissions set on specific objects |

## Key Rights to Understand

| Right | Purpose |
|---|---|
| Log on locally | Allows a user to sign in directly at the machine |
| Access this computer from the network | Allows remote connection without a local logon |
| Debug programs | Allows attaching a debugger to any process, including system processes |
| Act as part of the operating system | Extremely sensitive — Microsoft's guidance states no account should normally hold this right |
| Allow log on through Remote Desktop Services | Controls RDP access |

**Note:** Each user right has an associated constant name (e.g., `SeDebugPrivilege` for Debug programs) used when referencing the right in log events and security tooling.

## Key Concepts

- **Administrators** appears in almost every default right assignment — this is the underlying reason administrator accounts can perform nearly any action on a system.
- **Group inheritance:** when a user is a member of a group, they automatically receive that group's assigned rights and permissions.
- Rights can be configured at the local computer level or centrally via domain-level Group Policy Objects (GPOs) for consistency across many machines.

**Note:** Misconfigured or overly broad user rights are a common privilege escalation vector — for example, a standard user unintentionally granted "Debug programs" or "Act as part of the operating system" could escalate to SYSTEM-level access. From a SOC perspective, unexpected rights assignments are a notable indicator worth investigating during a security review.
