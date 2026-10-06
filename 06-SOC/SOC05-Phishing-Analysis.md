# SOC05: Phishing Analysis

Covers how a SOC analyst investigates, tools, and defends against phishing emails — from manual header analysis through automated tooling to realistic, multi-stage incident scenarios. Six rooms plus one SOC Simulator scenario: Phishing Analysis Fundamentals, Phishing Emails in Action, Phishing Analysis Tools, Phishing Prevention, The Greenholt Phish, Snapped Phish-ing Line, Phishing Unfolding.

## Phishing Analysis Fundamentals

A SOC analyst's role with phishing follows three steps: **Analyze** (True Positive or False Positive) → **Investigate** (headers and attachments) → **Remediate** (extract IOCs, update security tools).

### Key Email Headers

| Header | What It Shows |
|---|---|
| X-Originating-IP | The actual IP address the email was sent from |
| smtp.mailfrom / header.from | The domain the email claims to be sent from, found inside Authentication-Results |
| Reply-To | Where a reply actually goes — can differ from the visible sender address |
| Return-Path | Functionally equivalent to Reply-To |

### Email Authentication Protocols

| Protocol | Purpose |
|---|---|
| SPF (Sender Policy Framework) | Lists which IPs are authorized to send email for a domain; fails if the sending IP isn't listed |
| DKIM (DomainKeys Identified Mail) | Cryptographic signature verifying the email wasn't altered in transit |
| DMARC (Domain-based Message Authentication, Reporting, and Conformance) | Tells the receiving server what to do if SPF/DKIM fail: `p=none` (monitor only), `p=quarantine` (spam folder), `p=reject` (block outright) |

**Note:** These three protocols should always be checked together, not individually — SPF and DKIM provide the evidence, DMARC defines the enforcement action.

## Phishing Emails in Action

Practical pattern-recognition across real phishing samples, not just flagging emails as "suspicious."

| Technique | Description |
|---|---|
| Spoofed sender address | Display name looks legitimate, actual address doesn't match |
| URL shortening | Hides the real destination of a link |
| HTML brand impersonation | Email visually mimics a trusted company (PayPal, DHL, Netflix, Apple) |
| Urgency | Pressures the victim into acting fast without thinking |
| Tracking pixels | Invisible images confirming to the attacker when an email was opened |
| Malicious attachments | Some emails rely entirely on the attachment, with little or no body text |

**Note:** One sample in the room included an Excel attachment that, if macros were enabled, ran a malicious executable — a real-world example of macro-based delivery, the same mechanism covered conceptually in SEC04 (Malware Types).

## Phishing Analysis Tools

Automates the manual header/body analysis covered in earlier rooms.

| Tool | Purpose |
|---|---|
| CyberChef | "Extract URLs" recipe pulls links from an email body; also used to defang IPs/domains for safe reporting (e.g. `2[.]16[.]107[.]24`) |
| PhishTool | Automated analysis platform — parses an email and extracts headers, URLs, and attachment metadata in one pass; can connect to a VirusTotal API key for reputation checks |
| URL2PNG / Wannabrowser | Takes a safe screenshot or preview of a suspicious link without visiting it directly |
| Malware Sandbox (Any.Run) | Detonates a malicious attachment in an isolated environment and reports its behavior (e.g. flagging `svchost.exe` as "Potentially Bad Traffic") |

## Phishing Prevention

Shifts focus from detecting a single email to recommending organization-wide rules that prevent similar emails from reaching other employees.

**Note:** This directly reinforces the "Remediation Actions" field of the 5 Ws report structure — the deliverable here isn't just a verdict, it's a concrete defensive recommendation (e.g. block a domain, tighten SPF/DMARC enforcement).
## Module Takeaway

Fundamentals and Emails in Action build the technical foundation — header analysis, authentication protocols, and visual pattern recognition. Tools and Prevention add automation and a defensive, forward-looking deliverable. The final three rooms apply everything together in progressively realistic scenarios, ending with a live, evolving incident — mirroring how phishing investigations actually unfold in a real SOC rather than as a single clean sample.
