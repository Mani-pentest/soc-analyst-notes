# SOC04: Cyber Defence Frameworks

Covers the frameworks SOC analysts and threat hunters use to classify indicators, understand attacker behavior, and build durable detections. Six rooms: Pyramid of Pain, Cyber Kill Chain, Unified Kill Chain, MITRE, Summit, Eviction.

## Pyramid of Pain

A framework created by David Bianco ranking attack indicators by how much operational difficulty they cause an adversary when a defender detects and blocks them.

| Level | Indicator | Pain Caused to Attacker | Why |
|---|---|---|---|
| 1 (bottom) | Hash Values | Trivial | A single byte change produces a completely different hash — recompiling defeats it instantly |
| 2 | IP Addresses | Easy | Attackers rotate IPs easily, especially using Fast Flux (multiple IPs tied to one constantly-changing domain) |
| 3 | Domain Names | Simple | Harder than an IP, but still just a re-registration away |
| 4 | Network/Host Artifacts | Annoying | Forces the attacker to change tooling behavior, not just infrastructure |
| 5 | Tools | Challenging | Means building or acquiring new malware/tooling entirely |
| 6 (top) | TTPs (Tactics, Techniques, Procedures) | Tough | Forces the attacker to fundamentally change how they operate — the hardest thing to replace |

**Note:** Focusing detection efforts on hash values alone is nearly worthless, since evasion is trivial. The strategic takeaway is to build detections toward the top of the pyramid (TTPs), not the bottom (hashes), for defenses that remain effective over time.

## Cyber Kill Chain

Developed by Lockheed Martin in 2011, adapted from a military concept, breaking a cyberattack into seven sequential stages.

| Stage | Description |
|---|---|
| Reconnaissance | Attacker gathers information about the target (OSINT, email harvesting) |
| Weaponization | Combining malware and an exploit into a deliverable payload |
| Delivery | Moving the payload into the target environment |
| Exploitation | Triggering the payload by exploiting a vulnerability |
| Installation | Installing a backdoor or access point to establish persistence |
| Command & Control (C2) | Attacker establishes remote control over the compromised system |
| Actions on Objectives | Attacker achieves their actual goal, e.g. data theft or destruction |

**Note:** Each stage represents an opportunity for a defender to detect and disrupt the attack before the adversary progresses further — this is the origin of the phrase "breaking the kill chain."

## Unified Kill Chain (UKC)

Published by Paul Pols in 2017 and updated in 2022, designed to complement rather than compete with the Cyber Kill Chain and MITRE ATT&CK.

- 18 phases total, more granular than either of the frameworks above
- Grouped into three practical areas: **In** (Initial Foothold), **Through** (Network Propagation), **Out** (Action on Objectives)

| Phase | Corresponding MITRE Tactic |
|---|---|
| Social Engineering | TA0001 |
| Exploitation | TA0002 |
| Persistence | TA0003 |
| Defense Evasion | TA0005 |
| Credential Access / Privilege Escalation | TA0006 |
| Command & Control | TA0011 |
| Lateral Movement | TA0008 |
| Exfiltration | TA0010 |

**Note:** This framework maps directly onto real SOC scenarios — repeated failed logins followed by one success indicates the Privilege Escalation phase; a sudden spike in outbound traffic to an unknown IP indicates the Exfiltration phase.

## MITRE ATT&CK

MITRE created ATT&CK in 2013 to document and categorize the TTPs used by APT (Advanced Persistent Threat) groups against enterprise networks.

| Term | Definition |
|---|---|
| Tactic | The adversary's goal or objective — the "why" of an attack (e.g. Reconnaissance, Initial Access) |
| Technique | How the adversary achieves that goal (e.g. Active Scanning) |
| Sub-technique | A more specific method within a technique (e.g. Active Scanning breaks into Scanning IP Blocks, Vulnerability Scanning, Wordlist Scanning) |

| Resource | Purpose |
|---|---|
| ATT&CK Matrix | Visual layout of tactics across the top, with techniques nested beneath each |
| ATT&CK Navigator | Interactive tool to explore the matrix, map a specific threat group's TTPs, and visualize defensive coverage |

**Note:** Every technique page includes procedure examples (real groups and software observed using it), mitigations, detections, and references. IDs prefixed with "G" denote Groups, and IDs prefixed with "S" denote Software. MITRE also maintains related projects beyond ATT&CK: D3FEND (a defensive countermeasure framework), ENGAGE (an adversary engagement and deception framework), and CAR (the Cyber Analytics Repository). The Matrix is used by Red Teamers and SOC Managers as well as Blue Teamers.

## Summit (Practical Application)

A capstone scenario applying the Pyramid of Pain and MITRE ATT&CK together, chasing a simulated adversary up the pyramid until they abandon the attack.

**Flow, following the pyramid in order:**
1. Block by hash
2. Block by IP
3. Block by domain
4. Detect host/network artifacts using Sigma rules built against Sysmon logs
5. Force a change in the adversary's tooling
6. Force a change in the adversary's TTPs — the final, decisive stage

**Note:** Each detection in the room is mapped to a specific MITRE ATT&CK tactic. Disabling Windows Defender's real-time monitoring maps to Defense Evasion (TA0005); searching the compromised system maps to Discovery (TA0007); writing data to a file ahead of exfiltration maps to the Automated Exfiltration technique (T1020) under the Exfiltration tactic (TA0010). Once forced to the top of the pyramid, the adversary must rebuild their entire methodology to continue, which is why they ultimately give up.

## Eviction (Practical Application)

A proactive threat-hunting scenario: a SOC analyst must determine whether a known APT group has already compromised the network, using only the group's documented TTPs.

**Method:** use a pre-built MITRE ATT&CK Navigator layer for the specific APT group, then work tactic-by-tactic across the Navigator columns to reconstruct what the group's playbook looks like and check the environment against it.

**Examples from the scenario:**
- Resource Development tactic → technique identified: compromising Email Accounts
- Discovery tactic → technique identified: Network Sniffing (e.g., using tcpdump to discover other devices)
- Command and Control tactic → checked separately for the group's known infrastructure patterns

**Note:** This room demonstrates the practical, proactive use of MITRE ATT&CK — given only a group's name, an analyst can use the Navigator to reconstruct their likely attack path and hunt for evidence of compromise before or during a suspected intrusion, rather than only reacting after an alert fires.

## Module Takeaway

Pyramid of Pain determines which indicators are worth chasing. Cyber Kill Chain and Unified Kill Chain determine where in an attack's timeline a given event sits. MITRE ATT&CK provides the shared vocabulary — Tactics, Techniques, and Sub-techniques — to describe adversary behavior precisely and consistently across an organization. Summit and Eviction demonstrate these frameworks applied together: one from an offensive detection-building angle, the other from a proactive threat-hunting angle.
