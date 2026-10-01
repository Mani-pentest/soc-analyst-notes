# SOC02: SOC Team Internals

Covers the essential day-to-day skills of an L1 analyst: how alerts are triaged, how they're reported and escalated, what reference resources support that work, and how a SOC measures its own effectiveness. Four rooms plus the Introduction to Phishing SOC Simulator scenario.

## Alert Triage

Triage means reviewing an alert and deciding whether it's genuine (**True Positive**) or harmless (**False Positive**).

| Alert Property | Description |
|---|---|
| Time | When the alert fired |
| Severity | Low / Medium / High / Critical |
| Status | New, In Progress, Closed |
| Verdict | True Positive (TP) or False Positive (FP) |
| Assignee | Which analyst is handling it |

**Triage flow:** pick an unassigned alert → assign to yourself → check related events before and after the alert → decide TP/FP → close or escalate, with a documented reason.

**Note:** Closing an alert as a True Positive does not stop the attack by itself — it is only the first step; remediation and escalation still follow.

## Alert Reporting and Escalation

| Concept | Description |
|---|---|
| Alert Reporting | Documenting findings clearly enough that another analyst can understand the verdict without re-investigating |
| Escalation | Passing a confirmed or suspected True Positive that needs deeper investigation up to L2 |
| Escalation hierarchy | A critical threat is reported to L2 first, not directly to a manager |

**Note:** If a SIEM's logs are unparsed or unsearchable, the correct action is to investigate what's available and report the limitation to L2 or the SOC engineer — not to skip the alert.

## SOC Workbooks and Lookups

| Resource | Purpose |
|---|---|
| Workbook | A step-by-step guide for handling a specific alert type, used to structure and simplify triage |
| Lookup | A reference list (e.g., known-good IPs, approved domains) checked against during an investigation |
| In-house SOC | A SOC operated internally by the organization it protects |
| Managed SOC (MSSP) | A SOC operated by a third-party provider on behalf of client organizations |

## SOC Metrics and Objectives

The SOC's core goal: protect the **confidentiality, integrity, and availability** of the organization's digital assets.

| Metric | Definition |
|---|---|
| SLA (Service Level Agreement) | A signed agreement requiring quick detection, timely acknowledgement, and prompt response |
| MTTD (Mean Time to Detect) | Time from the attack occurring to detection by the SIEM |
| MTTA (Mean Time to Acknowledge) | Time from the alert firing to an analyst starting triage |
| MTTR (Mean Time to Respond) | Time to fully respond — e.g., isolating the affected device |

**Note:** A false positive rate above roughly 80% signals a need to automate triage or exclude trusted activity from detection rules. Zero alerts is not automatically good news — it can indicate a SIEM problem or a genuine blind spot.

## SOC Simulator: Introduction to Phishing

First hands-on scenario in the path — a guided phishing investigation inside TryHackMe's SOC Simulator, scored on both correct True/False Positive classification and the quality of the written report.

**Report structure used by the simulator (the practical application of the 5 Ws):**

| Field | Answers |
|---|---|
| Time of Activity | When |
| Affected Entities | Who and Where |
| Reason for True Positive | What and Why, with evidence |
| Reason for Escalating | Why it matters — the impact |
| Remediation Actions | What should happen next |
| Attack Indicators | Exact domains, URLs, IPs, hashes |

**Note:** Correct judgment and a well-written report are scored separately. Vague entities ("the user"), missing timestamps, and reasons that merely restate the verdict instead of citing evidence are the most common ways points are lost, even when the classification itself is correct.

**Section takeaway:** This module is where theory becomes a repeatable process: prioritize, triage, decide, document, escalate — then measure whether that process is working using MTTD, MTTA, MTTR, and false positive rate.
