# PROG-JS01: JavaScript for XSS Understanding

**Definition:** A scoped, minimum-necessary look at JavaScript — enough to understand *why* Cross-Site Scripting (XSS) attacks work, rather than general-purpose JavaScript programming.

---

## The `<script>` Tag

**Definition:** An HTML tag that instructs the browser to execute its contents as real code, rather than displaying them as plain text on the page.

```html
<script>alert("hello")</script>
```

- If this ends up embedded on a page — for example, via an unvalidated comment or input field — the browser executes it as code, not text.

---

## Why This Matters — XSS Core Concept

**Definition:** XSS (Cross-Site Scripting) occurs when a site fails to sanitize user input, allowing an attacker to inject executable script that runs in *other users'* browsers.

| Step | What happens |
|---|---|
| 1 | A site fails to sanitize user input |
| 2 | An attacker submits a `<script>` payload disguised as a "comment" |
| 3 | Another user views that page |
| 4 | That user's own browser executes the attacker's script — not the attacker's browser |

**Note:** This is what makes XSS dangerous: the vulnerable site becomes a delivery mechanism to attack other users, rather than just the attacker's own session.

---

## `document.cookie` — The Real Payload

**Definition:** A JavaScript property that exposes the current page's cookies, including session tokens.

```html
<script>document.cookie</script>
```

- Cookies store session tokens — proof that a browser is already logged in as a specific user.
- If an attacker's script captures and exfiltrates this token, they obtain the victim's session token directly.

---

## Session Hijacking

**Definition:** Using a stolen session token to impersonate a logged-in user, without ever obtaining their password.

| Fact | Detail |
|---|---|
| Password is never exposed | The attacker only ever sees the session token |
| Token is reused directly | The attacker pastes it into their own browser |
| No login step required | The website sees the token and assumes "this is User X, already logged in" |

---

## Why It Matters

This scoped JavaScript knowledge is what makes PortSwigger's XSS labs (Reflected, Stored, DOM XSS) make sense going forward — payloads aren't just copied and run blindly; the underlying mechanism (script execution → cookie theft → session hijack) is understood at each step.

## Summary

- `<script>` tags tell the browser to execute content as code, not display it as text
- XSS happens when unsanitized user input lets an attacker's script run in another user's browser
- The vulnerable site itself becomes the delivery mechanism — the victim's browser executes the payload, not the attacker's
- `document.cookie` exposes session tokens, which is the real target of most XSS payloads
- Session hijacking uses a stolen token to impersonate a user — no password is ever needed or seen
- This foundation directly supports understanding Reflected/Stored/DOM XSS labs later in the track
