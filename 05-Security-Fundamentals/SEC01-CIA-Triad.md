# SEC01: CIA Triad

**Definition:** The CIA Triad is the foundational security model — Confidentiality, Integrity, and Availability — three pillars that every security decision balances against each other.

---

## The Three Pillars

| Pillar | Definition | Real Example |
|---|---|---|
| Confidentiality | Only authorized parties can access data | BitLocker — data unreadable without the key, even if the device is stolen |
| Integrity | Data hasn't been tampered with or altered | Log tampering ("covering tracks") — an attacker deletes/edits logs to hide evidence |
| Availability | Systems/data are accessible when needed | VSS (Volume Shadow Copy) — snapshots allow recovery after corruption or ransomware |

---

## Why Integrity Matters for SOC Specifically

**Definition:** Integrity, in a SOC context, means the investigation timeline itself can be trusted — that logs accurately reflect what actually happened.

- If logs are tampered with, the investigation timeline is lost
- Without intact logs, key questions become unanswerable: "when did they get in," "what did they touch," "are they still here"
- This is why centralized log forwarding exists — logs are sent to a separate SIEM server immediately, so even if the original machine's logs are deleted, a copy survives elsewhere

---

## Key Insight: The Three Pillars Can Trade Off Against Each Other

**Example:** VSS supports availability (fast recovery from corruption or ransomware), but those same shadow copies can become a confidentiality risk if an attacker accesses old file versions through them.

**Note:** Real security decisions rarely satisfy all three pillars perfectly — most controls involve a deliberate balance, strengthening one pillar at some cost to another.

---

## Why It Matters

The CIA Triad isn't just a definitions exercise — it's the lens used to evaluate any security control or incident. Recognizing which pillar a control protects (and what it might weaken) is foundational to both SOC analysis and pentest reporting.

## Summary

- Confidentiality, Integrity, and Availability are the three core pillars security decisions are measured against
- Confidentiality: restricting access to authorized parties (e.g., BitLocker encryption)
- Integrity: ensuring data/logs haven't been altered (critical for SOC investigation timelines)
- Availability: ensuring systems/data remain accessible (e.g., VSS snapshots for recovery)
- Centralized log forwarding to a SIEM protects integrity by preserving a log copy even if the source machine is compromised
- The three pillars can conflict — a control that strengthens one (e.g., VSS for availability) can introduce risk to another (confidentiality via old file version access)
- Security controls typically represent a balance across the triad, not a perfect fit for all three
