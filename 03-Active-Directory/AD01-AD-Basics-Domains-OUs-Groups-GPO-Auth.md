# AD01: Active Directory Basics — Domains, DCs, OUs, Groups, GPOs, Authentication

Active Directory (AD) is the centralized repository Microsoft uses to manage users, computers, and resources across a business network. This chapter covers Windows Domains, AD DS, OUs, Groups, delegation, Group Policy, and authentication protocols (NetNTLM/Kerberos).

## Windows Domains & Active Directory

| Term | Meaning |
|---|---|
| Windows Domain | A group of users and computers under the administration of a business |
| Active Directory (AD) | Centralized repository storing common components of a Windows computer network |
| Domain Controller (DC) | The server running the Active Directory services |
| Active Directory Domain Service (AD DS) | The core catalogue holding information about all objects on the network |

**Note:** In a Windows domain, credentials are stored centrally in AD rather than on each individual machine — this enables centralized identity management across an entire organization.

## Why Use a Domain?

| Benefit | Description |
|---|---|
| Centralized Identity Management | All users across the network configured from AD with minimal effort |
| Centralized Security Policy Management | Security policies configured once in AD, applied across all users/computers |

## AD Objects

| Object | Description |
|---|---|
| Users | Represent people in the organization with specific privileges/access |
| Machines | Computer accounts — a machine named `TOM-PC` has an associated account `TOM-PC$` |
| Groups | Security principals that can hold privileges over network resources; can contain users and other groups |
| Shares/Printers | Network resources managed through AD |

## Organizational Units (OUs)

OUs are container objects used to classify users and machines, mainly for applying Group Policy consistently to sets of users with similar requirements (e.g., a new "Quality Assurance" department gets its own OU).

**Default containers created automatically:**

| Container | Purpose |
|---|---|
| Builtin | Default groups available to any Windows host |
| Computers | New machines joining the network land here by default |
| Domain Controllers | Default OU containing the DCs in the network |
| Users | Default users and groups applying domain-wide |

**Note:** OUs vs Groups — both classify users/computers, but OUs are for applying policy, while Groups are for granting permissions/privileges over resources.

## Delegation & Group Policy

| Concept | Description |
|---|---|
| Delegation | The process of granting a user privileges over a specific OU or AD object |
| Group Policy Objects (GPOs) | Collections of settings applied to OUs — can target either users or computers, setting a baseline configuration |

## Authentication Protocols

| Protocol | Key Fact |
|---|---|
| NetNTLM | Legacy authentication protocol — user's password is not transmitted over the network directly (a hash-based exchange is used instead) |
| Kerberos | Modern, preferred authentication protocol in current Windows versions — uses a Ticket Granting Ticket (TGT) to request further Ticket Granting Service (TGS) tickets |

**Note:** Kerberos is the default and preferred protocol on current Windows versions, not NetNTLM. This matters directly for Phase 4 — Kerberoasting specifically targets Kerberos TGS tickets.

## Domain Trusts

To let a user in Domain A access a resource in Domain B, a **trust relationship** must be configured between the two domains.
