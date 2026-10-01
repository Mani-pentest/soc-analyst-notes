# PY03: Regex & Web Scraping

**Definition:** Regex (regular expressions) is a pattern-matching language for finding specific text patterns inside larger blocks of text. Web scraping is the process of programmatically extracting data from a website's HTML.

---

## Regex Basics

| Symbol | Definition |
|---|---|
| `\d+` | Matches one or more digits in a row |
| `\.` | Matches a literal dot (backslash required — a plain `.` alone means "any character" in regex) |
| `re.findall(pattern, text)` | Searches text for every match of a pattern, returns all matches as a list (duplicates included) |

**Basic digit matching**
```python
import re
text = "The server IP is 192.168.1.5 and backup is 192.168.1.6"
matches = re.findall(r"\d+", text)
```
- `\d+` stops at every non-digit character, so it splits each IP apart instead of matching it whole
- Result: `['192', '168', '1', '5', '192', '168', '1', '6']`

---

## IP-Matching Pattern

**Definition:** A regex pattern built to match an entire IPv4 address as one single match, instead of breaking it into separate digit chunks.

```python
ip_pattern = r"\d+\.\d+\.\d+\.\d+"
```
- 4 digit-groups + 3 literal dots = matches the full IPv4 structure

**Bounded version (more accurate)**
```python
ip_pattern = r'\b(?:\d{1,3}\.){3}\d{1,3}\b'
```

| Part | Definition |
|---|---|
| `{1,3}` | Limits each digit group to 1–3 digits |
| `\b` | Word boundary — stops the pattern from matching digits inside a longer number |

- Result on example text: `['192.168.1.5', '192.168.1.6']`
- **Practical use:** Parsing IPs out of Nmap output, log files, and scan results

---

## Web Scraping — BeautifulSoup

**Definition:** BeautifulSoup is a Python library that parses raw HTML into a structure that can be searched and navigated, making it possible to extract specific elements (links, text, tags) from a webpage.

**Fetch + parse a page**
```python
import requests
from bs4 import BeautifulSoup

response = requests.get("http://example.com")
soup = BeautifulSoup(response.text, "html.parser")
print(soup.title)
```
- `response.text` — the raw HTML source of the page (same as "View Page Source")
- `BeautifulSoup(...)` — converts that raw HTML string into a navigable structure

**Extracting all links**
```python
links = soup.find_all("a")
for link in links:
    print(link.get("href"))
```
- `<a>` — the HTML tag used for a hyperlink
- `href` — the attribute holding the link's actual destination URL (not the visible text shown on the page)

---

## Why It Matters

Scraping every `href` off a site can reveal non-obvious pages (`/admin`, `/backup`, hidden subpages). Every reachable endpoint is potentially part of the attack surface, worth enumerating before deciding what to probe further.

## Summary

- `\d+` matches digit runs but breaks at non-digit characters, splitting IPs into separate chunks unless bounded properly
- `\.` escapes the dot to match it literally, since a plain `.` matches any character in regex
- `re.findall()` returns every match in a list, including duplicates
- The bounded IP pattern (`\b(?:\d{1,3}\.){3}\d{1,3}\b`) correctly matches whole IPv4 addresses as single results
- `requests` + `response.text` fetches raw HTML; `BeautifulSoup` parses it into a searchable structure
- `soup.find_all("a")` + `.get("href")` extracts every link's destination URL from a page
- Scraping `href` values off a site is a practical recon technique for discovering non-obvious endpoints and expanding the attack surface
