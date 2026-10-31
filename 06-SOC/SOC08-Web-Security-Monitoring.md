# SOC08: Web Security Monitoring

Covers how a SOC analyst monitors web applications and recognizes web attacks in access logs, network traffic, and the file system. Four rooms: Web Security Essentials, Detecting Web Attacks, Detecting Web Shells, Detecting Web DDoS.

## Web Security Essentials

Web security is layered. Each layer has its own controls and its own logs.

| Layer | Focus | Key Controls |
|---|---|---|
| Web application | The code | Access control, safe error messages, input validation against injection |
| Web server | The server software | Hardening, WAF, logging |
| Host machine | The OS underneath | Patching, monitoring, file integrity |

A Web Application Firewall (WAF) inspects incoming HTTP requests and blocks or logs harmful ones based on rules. It matches requests against known malicious patterns. Many CDNs, such as Cloudflare, Amazon CloudFront, and Azure Front Door, include a WAF.

**Note:** A WAF is one layer, not a replacement for secure code. Detailed error messages leak information to attackers, and a WAF cannot fix broken access control inside the application.

Web server logs (access log and error log) record every request, so they are the main evidence source for web investigations.

## Detecting Web Attacks

| Class | Meaning | Example |
|---|---|---|
| Client-side | Targets the user's browser or behavior | Cross-Site Scripting (XSS) |
| Server-side | Targets the web server or its database | SQL injection (SQLi), brute force, directory fuzzing |

SQL injection works when an application builds queries by string concatenation instead of parameterized queries, so an attacker can change the intended SQL command.

An access log entry normally contains the source IP, timestamp, HTTP method, requested path and query string, status code, response size, referrer, and User-Agent.

| Attack | What It Looks Like in the Access Log |
|---|---|
| Directory fuzzing | Many requests from one IP to different paths, mostly 404; a fuzzing tool in the User-Agent (for example ffuf) |
| Brute force | Many POST requests to a login page from one IP; a change in response (status or size) when one succeeds |
| SQL injection | Quotes, `OR 1=1`, `UNION SELECT` in parameters, often URL-encoded; sqlmap in the User-Agent |
| XSS | `<script>` or `onerror=` in parameters |
| Path traversal | `../` sequences in the path |

| Command / Tool | Purpose |
|---|---|
| `grep "<IP>" access.log` | Show everything one IP did |
| `grep "<IP>" access.log \| grep "200"` | Show which of its requests succeeded |
| CyberChef (URL Decode) | Decode a payload to read it |
| Add User-Agent as a column | Spot tools and scripted traffic quickly |
| Wireshark: `http.request.method == "POST"` | Find brute-force attempts in a PCAP |

**Note:** Logs have limits. Some formats do not record POST bodies or full query strings, so combine access logs with network captures and WAF logs. A custom WAF rule can block a known bad User-Agent, but User-Agents are easy to change.

## Detecting Web Shells

A web shell is a malicious script uploaded to a web server that lets an attacker run commands through HTTP requests. It gives the attacker persistence and often blends in with the legitimate application, which makes it hard to spot. Some run entirely in memory, so file-based detection can miss them.

MITRE ATT&CK: T1505.003 (Server Software Component: Web Shell). Common extensions: `.php`, `.jsp`, and `.aspx` (commonly used against Microsoft Exchange).

| Detection Area | What to Look For |
|---|---|
| Access log | POST or PUT requests that upload a file; repeated GET or POST requests to the same file; query strings carrying commands; requests from unusual external IPs |
| File system | New or recently modified script files in upload or web directories; suspicious functions such as `eval(` |
| Network traffic | Upload requests (PUT or POST) and command text in the payload |
| Behavior | The web server process starting system commands or making outbound connections |

| Command | Purpose |
|---|---|
| `grep ".php" /var/log/apache2/access.log` | Find requests to PHP files |
| `grep "<file>.php" /var/log/apache2/access.log` | Show all activity on one suspect file |
| `find /var/www/ -type f -name "*.php"` | List all PHP files in the web root |
| `grep -r "eval(" wp-content` | Find files that run dynamic code |
| `ausearch -k web_shell` | Search auditd events tagged for web shell activity |
| Wireshark: `http.request.method == "PUT"` | Find file uploads in a capture |

**Note:** Web shell traffic is often encoded or encrypted, so network signatures only partly work. The strongest approach is correlation: an upload in the access log, then a new file on disk, then command requests to that file.

## Detecting Web DDoS

A Denial-of-Service attack aims to make a service unavailable. A Distributed attack (DDoS) uses many sources, usually a botnet. Motives include extortion, reputational damage, and hacktivism. This room focuses on Layer 7 (application layer) attacks.

| Type | Layer | Mechanism | Example |
|---|---|---|---|
| Volumetric | L3/L4 | Floods bandwidth | UDP or ICMP flood |
| Protocol | L4 | Exhausts connection state | SYN flood |
| Application | L7 | Exhausts application resources | HTTP flood, Slowloris |

Slowloris sends partial HTTP requests and keeps connections open, exhausting the connection pool with very little traffic. Application-layer floods are hard to detect because the requests look legitimate.

| Log Sign | Meaning |
|---|---|
| High request rate to one page (often `/login` or `/search`) | Flood targeting an expensive endpoint |
| Burst timestamps | Dozens of requests per second from one IP |
| Odd User-Agents (curl, Python, Java) | Scripted or bot traffic |
| Geographic anomaly | Sudden traffic from regions you do not normally serve |
| 503 responses to normal users | The service is overloaded |

| Splunk Search | Purpose |
|---|---|
| `... \| stats count by clientip \| sort -count` | Rank the busiest source IPs |
| `... \| stats count by uri \| sort -count` | Find the most-requested URL |
| `... \| timechart span=1s count` | Show the attack peak per second |

| Mitigation | How It Helps |
|---|---|
| Rate limiting | Caps requests per IP |
| WAF / CDN | Absorbs and filters traffic at the edge |
| CAPTCHA | Blocks simple bots |
| Load balancing | Spreads traffic across servers |

**Note:** Also check the time of the first 503 responses. They mark when the attack started to affect real users.

## Investigation Workflow

| Step | Action |
|---|---|
| Identify | Find the source IP, the targeted URL, and the time window from the alert |
| Trace | List everything that IP did in the access log |
| Classify the activity | Fuzzing, brute force, injection, web shell, or flood |
| Confirm | Check a second source: network capture, WAF log, or the file system |
| Report | Write the 5 Ws report with the evidence and defanged indicators |

**Note:** A single log line is rarely enough. For a suspected web shell, tie together the upload request, the new file on disk, and the requests that follow it.

## Module Takeaway

Web Security Essentials gives the layers and what a WAF does. Detecting Web Attacks teaches the log footprints of fuzzing, brute force, SQL injection, and XSS. Detecting Web Shells shows persistence through uploaded scripts and how to find them in logs, on disk, and in traffic. Detecting Web DDoS covers availability attacks and the request-rate patterns that reveal them. When a web alert arrives, find the source IP, trace its requests in the access log, decide what it was trying to do, and confirm with a second source before you report.
