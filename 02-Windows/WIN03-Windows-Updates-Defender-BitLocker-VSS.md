# WIN03: Windows Fundamentals 3 — Updates, Defender, Firewall, BitLocker, VSS

## Windows Update
Microsoft service providing security patches, feature enhancements, and updates. Released monthly (2nd Tuesday), with urgent patches pushed as needed. Updates can be postponed but not permanently ignored.

## Windows Security (Defender)

| Feature | Purpose |
|---|---|
| Virus & Threat Protection | Real-time malware scanning |
| Real-time Protection | Continuously monitors for threats as they occur |
| Firewall & Network Protection | Distinguishes network types (e.g., Public network) to apply appropriate rules |

**Note:** "Green" status in Windows Security means the device is sufficiently protected with no recommended actions — a quick health-check indicator worth knowing for triage.

## TPM (Trusted Platform Module)
A secure crypto-processor chip designed to carry out cryptographic operations — underpins BitLocker's strongest security mode.

## BitLocker
Data protection feature that encrypts drives, protecting against data theft/exposure from lost, stolen, or improperly decommissioned devices.

**Note:** On computers without TPM 1.2+, BitLocker still works but requires a **USB startup key** inserted at boot or resume from hibernation.

## Volume Shadow Copy Service (VSS)
Coordinates creation of consistent shadow copies (snapshots) of data for backup, without taking the application offline. Introduced in Windows Server 2003.

**Note:** Malware authors are often aware of VSS and have techniques to delete/evade shadow copies (common in ransomware) — this is why offline backups remain important, and why VSS deletion is a notable IOC (indicator of compromise) in SOC investigations.
