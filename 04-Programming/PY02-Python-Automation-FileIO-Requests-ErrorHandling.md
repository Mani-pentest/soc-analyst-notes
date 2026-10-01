# PY02: Python for Automation — File I/O, Requests, Error Handling

This chapter covers reading files, making HTTP requests with the `requests` library, and handling errors gracefully — the core skills needed to write simple recon and automation scripts.

## Reading Files

Files are opened using `open()` with a mode: `"r"` for read, `"w"` for write (which overwrites existing content).

```python
with open("targets.txt", "r") as f:
    for line in f:
        print(line.strip())
```

**Note:** Each line read from a file includes a trailing newline character (`\n`), and `print()` adds its own newline by default — without `.strip()`, this produces a blank line after every output line. `.strip()` removes leading/trailing whitespace, including `\n`.

## Combining File Reading with Conditionals

```python
with open("targets.txt", "r") as file:
    for line in file:
        ip = line.strip()
        if ip.startswith("192.168"):
            print(ip)
```

## The `requests` Library

`requests` allows Python to send HTTP requests and inspect responses — directly useful for checking site availability during recon.

```python
import requests
response = requests.get("http://example.com")
print(response.status_code)
```

**Note:** Status code `200` indicates success; `404` indicates the resource was not found — consistent with standard HTTP status code conventions.

## Error Handling with `try` / `except`

Unlike `if`/`else`, which evaluates a known condition, `try`/`except` catches errors that occur during code execution — such as a connection failure or timeout — preventing the entire script from crashing.

```python
import requests
urls = ["https://example.com", "https://example.org", "https://example.net"]

for url in urls:
    try:
        response = requests.get(url, timeout=10)
        if response.status_code == 200:
            print(f"{url}: UP")
        else:
            print(f"{url}: DOWN - HTTP {response.status_code}")
    except Exception as e:
        print(f"{url}: DOWN - {e}")
```

**Note:** `except Exception as e` captures the specific error message, useful for logging what actually went wrong (timeout, DNS failure, connection refused, etc.), rather than just knowing that *something* failed.

**Note:** Setting a `timeout` parameter (e.g., `timeout=10`) prevents a request from hanging indefinitely on an unresponsive host — an important practical consideration for any recon script checking multiple targets.
