# SEC03: OWASP Top 10:2025 — Overview

**Definition:** OWASP (Open Web Application Security Project) periodically surveys real-world vulnerability data across thousands of applications and publishes a ranked list of the 10 most critical web application security risks.

**⚠️ Important:** This is the NEW 2025 list (final release Jan 2026) — it replaces the older 2021 list. Two brand-new categories were added and several existing categories were re-ranked. Older study material (TryHackMe rooms, blog posts) may still reference the outdated 2021 version.

---

## Full List

| Rank | Category | Note |
|---|---|---|
| A01 | Broken Access Control | #1 again — SSRF now absorbed into this category |
| A02 | Security Misconfiguration | Jumped #5 → #2 |
| A03 | Software Supply Chain Failures | NEW — expanded from "Vulnerable Components" |
| A04 | Cryptographic Failures | Dropped #2 → #4 |
| A05 | Injection | Dropped #3 → #5 |
| A06 | Insecure Design | Dropped #4 → #6 |
| A07 | Authentication Failures | Same #7, renamed |
| A08 | Software or Data Integrity Failures | Same #8 |
| A09 | Security Logging & Alerting Failures | Same #9, renamed (added "Alerting") |
| A10 | Mishandling of Exceptional Conditions | NEW category |

---

## Definitions

**A01 — Broken Access Control**
The system fails to enforce what a user is allowed to do. Example: IDOR (Insecure Direct Object Reference) — changing `?user_id=123` to `?user_id=124` to view another user's data. Now includes SSRF (Server-Side Request Forgery — tricking a server into making unauthorized requests on the attacker's behalf).

**A02 — Security Misconfiguration**
Default settings left unchanged, unnecessary features enabled, missing hardening. Mostly a problem of inaction rather than an active mistake — more configuration options in modern software means a larger attack surface.

**A03 — Software Supply Chain Failures**
Compromised or vulnerable third-party dependencies, build systems, or distribution infrastructure — risk introduced by code the developer didn't write themselves (`pip install`, `npm install`).

**A04 — Cryptographic Failures**
Weak or missing encryption that exposes sensitive data.

**A05 — Injection**
Untrusted input treated as executable code (SQL injection, XSS, command injection). Same underlying mechanism as `<script>` tag execution covered in the JavaScript/XSS notes.

**A06 — Insecure Design**
The flaw exists in the architecture itself, not in a coding bug. Example: no rate-limiting on password reset, by design — the code works exactly as designed, but the design didn't account for abuse.

**A07 — Authentication Failures**
Weak login mechanisms — brute-forceable passwords, no MFA, broken session handling.

**A08 — Software or Data Integrity Failures**
Trusting unsigned or unverified code or data updates at the point of use.

**A09 — Security Logging & Alerting Failures**
Logs not captured, or nobody alerted on suspicious activity within them, so attacks go undetected. Ties directly to the Integrity pillar and log-tampering discussion from SEC01.

**A10 — Mishandling of Exceptional Conditions**
Improper error handling — logic that "fails open" (accidentally grants access or leaks details on error) instead of failing safely (denies by default).

---

## Why It Matters

This list is the shared vocabulary the security industry uses to categorize and prioritize web vulnerabilities. Pentest findings are commonly framed using these exact category names (e.g., "this finding falls under A01: Broken Access Control") because it immediately communicates severity and context without re-explaining the concept each time. The list also unifies several concepts already covered elsewhere in this track: misconfiguration (A02) connects to SUID/WIN04 findings, injection (A05) connects to the `<script>` tag mechanism, and logging failures (A09) connect to the Integrity pillar from SEC01.

## Summary

- The OWASP Top 10 is based on real-world vulnerability data, not theory, and is periodically revised — always confirm which version (2021 vs 2025) a source is referencing
- A01 Broken Access Control remains #1 and now absorbs SSRF
- A02 Security Misconfiguration jumped sharply (#5 → #2) as modern software's configuration surface has grown
- A03 Software Supply Chain Failures is new, reflecting growing risk from third-party dependencies
- A04 Cryptographic Failures and A05 Injection both dropped in rank, likely reflecting broader industry maturity in those areas relative to others
- A06 Insecure Design is distinct from a coding bug — it requires redesigning the approach, not just patching code
- A09's rename (adding "Alerting") emphasizes that logs must be actively monitored, not just passively collected
- A10 Mishandling of Exceptional Conditions is new, covering "fail open" vs "fail safe" error-handling design choices
