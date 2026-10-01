# SEC06: Web Application Basics — Structure, HTTP Messages, Requests/Responses, Security Headers

This chapter covers the components of a web application, URL structure, the anatomy of HTTP requests and responses, and the security headers that protect modern web applications.

## Web Application Components

| Layer | Contains | Role |
|---|---|---|
| Front End | HTML, CSS, JavaScript | What the user sees and interacts with in the browser |
| Back End | Database, Web Server, Infrastructure | Invisible components enabling the application to function |
| WAF (Web Application Firewall) | — | Optional component that filters malicious requests before they reach the server |

## URL Anatomy

A URL is structured as: `scheme://user@host:port/path?query#fragment`

| Component | Description |
|---|---|
| Scheme | Protocol used (HTTP or HTTPS) — HTTPS encrypts the connection |
| Host/Domain | Identifies which website is being accessed |
| Port | Directs to the correct service on the server (default 80 for HTTP, 443 for HTTPS) |
| Path | Specific resource being requested |
| Query String | Extra data passed via `?key=value`, vulnerable to injection if unsanitized |
| Fragment | References a specific section of the page via `#` |

**Note:** Typosquatting refers to registering misspelled variations of legitimate domain names to exploit user error, often for fraudulent purposes.

## HTTP Message Structure

Every HTTP message (request or response) follows the same structure: **Start Line → Headers → Empty Line → Body**. The empty line separates headers from the body and is essential for correct parsing.

## HTTP Request Methods

| Method | Purpose | Security Consideration |
|---|---|---|
| GET | Retrieves data without making changes | Should never carry sensitive data (passwords, tokens) since it may be logged in plaintext |
| POST | Sends data to create or update a resource | Requires input validation to prevent SQL injection or XSS |
| PUT | Replaces or updates a resource | Requires authorization checks before accepting |
| DELETE | Removes a resource | Requires authorization checks before accepting |
| PATCH | Partially updates a resource | Requires validation to avoid data inconsistencies |
| HEAD | Retrieves headers only, no body | Useful for checking metadata without downloading content |
| OPTIONS | Lists methods supported by a resource | Should be disabled if not required, to reduce attack surface |
| TRACE | Echoes the received request, used for debugging | Frequently disabled in production for security reasons |
| CONNECT | Establishes a secure tunnel, used for HTTPS | Critical for encrypted communication |

## Request Body Formats

| Format | Content-Type | Structure |
|---|---|---|
| URL Encoded | `application/x-www-form-urlencoded` | `key=value&key2=value2` |
| Form Data | `multipart/form-data` | Multiple blocks separated by a boundary string; supports binary data (file uploads) |
| JSON | `application/json` | `{"key": "value"}` |
| XML | `application/xml` | `<tag>value</tag>` |

## HTTP Response Status Codes

| Range | Category |
|---|---|
| 100–199 | Informational |
| 200–299 | Success |
| 300–399 | Redirection |
| 400–499 | Client Error |
| 500–599 | Server Error |

## Response Headers — Security Relevance

| Header | Note |
|---|---|
| Server | Reveals the server software/version in use — can expose an information disclosure risk if left unobscured |
| Set-Cookie | Sends a cookie to the client; should be configured with the **HttpOnly** flag (prevents JavaScript access, mitigating XSS-based cookie theft — see PROG-JS01) and **Secure** flag (restricts transmission to HTTPS) |
| Location | Used in redirect (3xx) responses; if user-modifiable and unsanitized, can lead to open redirect vulnerabilities |

## Security Headers

| Header | Purpose |
|---|---|
| Content-Security-Policy (CSP) | Restricts which domains/sources are permitted to serve scripts, styles, and other content, mitigating XSS attacks |
| Strict-Transport-Security (HSTS) | Forces browsers to connect only over HTTPS for the specified duration |
| X-Content-Type-Options: nosniff | Prevents browsers from guessing (sniffing) a resource's MIME type, reducing content-type-based attacks |
| Referrer-Policy | Controls how much referring-page information is shared when navigating to another site |

**Note:** The HttpOnly cookie flag directly mitigates the session hijacking mechanism covered in PROG-JS01 — if JavaScript cannot access `document.cookie` due to HttpOnly being set, an XSS payload cannot steal the session token through that vector.

## Why It Matters

This chapter ties together nearly everything covered so far into how real web traffic actually works: the `<script>`/session-hijacking mechanism from PROG-JS01 explains *why* HttpOnly matters, the OWASP injection category (SEC03) explains *why* query strings and POST bodies need validation, and understanding status codes and headers is foundational to reading Burp Suite traffic in upcoming hands-on labs.

## Summary

- A web app splits into a front end (HTML/CSS/JS) and back end (database, server, infrastructure), optionally protected by a WAF
- URLs break into scheme, host, port, path, query string, and fragment — query strings are a common injection point
- Every HTTP message follows Start Line → Headers → Empty Line → Body
- HTTP methods each carry distinct security considerations — GET must never carry secrets, POST/PUT/PATCH/DELETE need validation and authorization checks
- Status codes group into five ranges: informational, success, redirection, client error, server error
- `Set-Cookie` should carry HttpOnly (blocks JS access, mitigating XSS cookie theft) and Secure (HTTPS-only) flags
- Security headers like CSP, HSTS, X-Content-Type-Options, and Referrer-Policy each close off a specific class of attack
