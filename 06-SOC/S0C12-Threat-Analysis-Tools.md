# SOC12: Threat Analysis Tools

Covers how a SOC analyst turns a raw indicator into a decision: what threat intelligence is and how it is classified and shared, how to enrich a file hash, how to enrich an IP address and a domain, and how to pivot across indicators in an investigation. Four rooms: Intro to Cyber Threat Intel, File and Hash Threat Intel, IP and Domain Threat Intel, Invite Only.

## Intro to Cyber Threat Intel

Cyber threat intelligence (CTI) is evidence-based knowledge about adversaries and their behaviour that helps an analyst decide what an alert means and what to do about it. It answers three questions: who or what is behind this indicator, what have they done before, and how should the organisation respond right now.

| Level | What It Is | Example |
|---|---|---|
| Data | An unprocessed observable | An IP address and port |
| Information | Data with factual context | The IP is registered to a hosting provider and was first seen on a date |
| Intelligence | Analysed information that answers "so what" and drives an action | The IP belongs to a known malware command server, so block it and escalate |

The analyst's job at Level 1 is to enrich artefacts until they qualify as intelligence, or to show that they never will.

| Indicator | Examples of Where to Enrich It | Related Behaviour (IOA) |
|---|---|---|
| IP address | WHOIS for ASN and allocation date, VirusTotal relations, Shodan | Repeated SSH failures, T1110.003 Password Guessing |
| Domain | WHOIS age, passive DNS, urlscan.io | A surge of DNS queries to a very new domain |
| URL | URLhaus, urlscan.io, a sandbox with the network off | A browser POST to an unusual path |
| Email address | Header analysis, breach lookups | SPF failure plus a recently registered sender domain |

**Note:** An IOC is evidence of a compromise, such as a hash or an IP. An IOA is behaviour that suggests an attack is in progress. Analysts escalate technical IOCs and document the behaviour patterns.

| Source Type | Examples |
|---|---|
| Internal | Logs, incident reports, vulnerability scans |
| Open source (OSINT) | AbuseIPDB, URLhaus, public blogs and reports with indicators |
| Communities and ISACs | Sector-specific sharing groups |
| Commercial | Paid feeds and vendor reports |

**Note:** Open-source intel must be cross-confirmed before it drives an action. A single feed is not proof.

| Classification | Focus | Typical Reader |
|---|---|---|
| Strategic | High-level trends and risks that guide business decisions | Executives |
| Technical | Atomic indicators and artefacts such as IPs and hashes | SOC and detection teams |
| Tactical | Adversary tactics, techniques, and procedures (TTPs) | Analysts and detection engineers |
| Operational | Campaign details, motives, and intent | Incident response and threat hunters |

| Lifecycle Phase | What Happens |
|---|---|
| Direction | Define objectives, critical assets, and the questions to answer |
| Collection | Gather data from internal and external sources, often automated |
| Processing | Sort, organise, correlate, and tag the raw data (a SIEM helps here) |
| Analysis | Find patterns and indicators, decide what they mean |
| Dissemination | Deliver the right level of detail to each audience |
| Feedback | Collect stakeholder input and improve the cycle |

The Traffic Light Protocol (TLP) labels how widely a piece of intel may be shared.

| TLP Label | Sharing Boundary |
|---|---|
| Red | Named recipients only |
| Amber | The organisation and those who need to know |
| Green | The wider community, not public |
| Clear | No restriction |

**Note:** Breaking a TLP label can breach agreements and damage trust with the partner who shared the intel. Check the label before forwarding anything.

Intelligence is shared through standard formats and platforms. STIX is a common language for describing threats, TAXII is the transport that moves it, and a threat intel platform acts as one place to query indicators first. New feeds should be added gradually and judged on whether they produce actionable results.

## File and Hash Threat Intel

This room teaches how to enrich a newly seen file using its name, path, hash, and reputation, then confirm its behaviour in a sandbox report. The lab uses an offline threat intel tool that mimics VirusTotal so the data does not change between attempts.

| Step | Action |
|---|---|
| Filename and path | Look for heuristics: generic installer names, double extensions, random names, system-looking names in the wrong folder, files in temp or user-writable paths |
| Hash | Generate a SHA256 and search it. Any byte change gives a different hash |
| Reputation | Check detections, first submission date, file type, and community comments |
| Relations | Check parent files, files it dropped, and addresses it contacts |
| Behaviour | Read the sandbox report for processes, files, registry changes, and network |
| Decide | Malicious, suspicious, or benign, and what to do next |

| Command | Purpose |
|---|---|
| `certutil -hashfile <file> SHA256` | Hash a file on Windows |
| `Get-FileHash <file> -Algorithm SHA256` | Hash a file in PowerShell |
| `sha256sum <file>` | Hash a file on Linux |

| VirusTotal Area | What It Tells You |
|---|---|
| Detection | How many vendors flag the file and under which names |
| Details | File type, names seen, first submission, signature information |
| Relations | Parent files, dropped files, and contacted addresses |
| Behaviour | Sandbox activity and the MITRE techniques it maps to |
| Community | Analyst comments and context |

| Source | Why It Helps |
|---|---|
| VirusTotal | Aggregated vendor results and behaviour for a hash |
| MalwareBazaar | Malware family tags, YARA matches, vendor classifications, and downloadable samples |
| Hybrid Analysis or another sandbox | Dynamic behaviour: dropped files, executed scripts, network connections |

**Note:** A low detection count does not mean a file is safe. A file with few detections but a malware family tag on MalwareBazaar should be treated as malicious. Correlate with endpoint logs, because some admin tools change the registry legitimately.

## IP and Domain Threat Intel

The runbook idea for this room is verify, enrich, decide. For an IP or domain you pivot on registration data, DNS, location, ownership, exposed services, and reputation, and you correlate across sources instead of trusting one verdict.

| Domain Check | What to Look For |
|---|---|
| WHOIS | Registration date, registrar, and ownership details. A very new domain is a red flag |
| DNS records | A and AAAA (addresses), NS (name servers), MX (mail), TXT (SPF and verification), SOA, and TTL values |
| DNS pattern | Frequently changing addresses or very short TTLs can mean throwaway infrastructure. Compare with normal CDN behaviour |
| Passive DNS and certificates | Past resolutions and certificate history show related infrastructure |

| IP Check | What to Look For |
|---|---|
| Geolocation | The country, which matters for context and legal escalation |
| ASN | The network owner. A hosting provider is different from a home ISP |
| Reputation | Vendor and community verdicts, for example on VirusTotal |
| Service exposure | Shodan or Censys banners showing open services, unusual ports, or remote-access tools |
| VPN, proxy, and Tor | Services such as IP2Proxy and Spur show whether the address hides the real source |

**Note:** Geolocation can be wrong and a VPN can hide the real country. Use location as a clue, not a verdict.

## Invite Only

Invite Only is a challenge room. A Level 1 analyst flags two indicators, an IP address and a SHA256 hash, and the task is to turn them into usable threat intelligence with the lab's in-house threat intel tool.

| Step | What You Do |
|---|---|
| Identify the file | Search the hash and record the file name and type |
| Find the execution chain | Check which parent files ran it and what each one dropped |
| Record hashes | Note the hash of each related file for the next lookup |
| Pivot on the IP | Review the files linked to the flagged address to see what they share |
| Name the family | Decide which malware family connects the files |
| Use OSINT | Find the public report that documents these indicators |
| Extract the story | Record the tools used, the social engineering method, and the platform used to deliver the victim |

**Note:** The skill here is pivoting: one hash leads to parents, dropped files, an IP, a family, and a report. Each pivot adds context until the raw indicator becomes intelligence you can write up.

## Indicator Enrichment Workflow

| Step | Action |
|---|---|
| Extract | Pull the indicator from the alert: hash, IP, domain, or URL |
| Defang | Record it in a safe form such as `example[.]com` |
| Enrich | Use the matching source: VirusTotal and MalwareBazaar for files, WHOIS, DNS, Shodan, and reputation tools for IPs and domains |
| Correlate | Compare at least two independent sources |
| Context | Check the TLP label, how new the indicator is, and whether it appears elsewhere in your logs |
| Decide | Benign, suspicious, or malicious, with the reason |
| Act and record | Block, escalate, or close, and write the finding with the evidence |

## MITRE ATT&CK Coverage

| Technique | ID | Where It Appears |
|---|---|---|
| Password Guessing | T1110.003 | Intro to CTI indicator example |
| Visual Basic | T1059.005 | Intro to CTI tactical intel example |
| Process Injection | T1055 | Intro to CTI file example |
| Masquerading | T1036 | File and Hash filename heuristics |

## Common Mistakes

- Trusting one source or one verdict instead of correlating
- Treating a low detection count as proof a file is safe
- Judging an IP by its country alone
- Forgetting to defang indicators in reports and tickets
- Ignoring the TLP label before sharing intel
- Blocking an indicator without checking whether it is a shared service such as a CDN

## Module Takeaway

Intro to Cyber Threat Intel gives the vocabulary: data becomes information and then intelligence, intel is classified, collected through a six-phase cycle, and shared under TLP rules. File and Hash Threat Intel shows how to enrich a file in order: name and path, hash, reputation, relations, then sandbox behaviour. IP and Domain Threat Intel does the same for network indicators using WHOIS age, DNS records, ASN, exposed services, and VPN or proxy detection. Invite Only puts it together by pivoting from two indicators to a malware family and a public report. When an indicator appears in an alert, extract it, defang it, enrich it from more than one source, then decide and document.
