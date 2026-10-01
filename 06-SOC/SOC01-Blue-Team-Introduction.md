# SOC01: Blue Team Introduction

Covers the foundational mindset of SOC work: the analyst's daily workflow, where SOC sits within the Blue Team, and the two broad categories of attack vectors a SOC defends against. Four rooms: Junior Security Analyst Intro, SOC Role in Blue Team, Humans as Attack Vectors, Systems as Attack Vectors.

## Blue Team vs Red Team

| Term | Definition |
|---|---|
| Blue Team | The defensive side of cybersecurity — protects, detects, and responds to threats |
| Red Team | The offensive side — simulates attacks to test defenses |
| SOC (Security Operations Center) | The team and function within Blue Team responsible for continuous monitoring, detection, and response |

## The L1 Analyst Workflow

A SOC analyst's core loop: **Alert received → Investigate → Decide True/False Positive → Escalate or Close → Document.**

## Analyst Tiers

| Tier | Role |
|---|---|
| L1 | Triage — first responder, filters noise from real threats |
| L2 | Investigation — deeper analysis of escalated True Positives |
| L3 | Threat Hunting — proactive search for undetected threats |

**Note:** Career progression in a SOC runs L1 → L2 → L3, typically earned through consistent performance metrics and demonstrated investigative judgment, not lateral specialization like a software team.

## Humans as Attack Vectors

People are the most commonly exploited entry point into an organization, not just technical systems.

| Concept | Description |
|---|---|
| Social Engineering | Manipulating people into bypassing security controls (phishing, pretexting, baiting) |
| Why it works | Exploits trust, urgency, and the instinct to be helpful — not a technical flaw |
| SOC's role | User awareness training, plus detecting the technical trail social engineering leaves behind (phishing emails, credential misuse) |

## Systems as Attack Vectors

Attackers also target misconfigured or unpatched systems directly.

| Concept | Description |
|---|---|
| Attack Surface | Every system/service exposed that could potentially be exploited |
| Common weaknesses | Default credentials, missing patches, unnecessary open ports/services |
| SOC's role | Monitor for exploitation attempts, advocate for timely patching and hardening |

**Section takeaway:** SOC exists because both humans and systems are exploitable. Everything covered later in this path — SIEM, frameworks, detection tooling — is the practical *how* behind this foundational *why*.
