# Application Attacks

## Overview

Applications are a primary attack surface in modern systems. When developers fail to validate input, enforce access controls, or properly configure their servers, attackers can exploit those gaps to read data, escalate privileges, forge requests, or traverse file systems. This page covers several key application attack types and their defenses.

> **Related docs:** [SQL Injection](sql-injection.md) · [Buffer Overflow](buffer-overflow.md) · [Cross-Site Scripting](cross-site-scripting.md) · [Replay Attacks](replay-attacks-and-session-hijacking.md)

---

## Privilege Escalation

When a user logs into an application, they receive permissions appropriate to their role. **Privilege escalation** occurs when an attacker exploits a vulnerability to gain higher permissions than they should have.

### Types of Privilege Escalation

| Type | Description |
|------|-------------|
| **Vertical escalation** | Moving from a low-privilege account to a higher one — ideally administrator or system level |
| **Horizontal escalation** | Moving laterally from one user's account to another user's account at the same privilege level |

### Real-World Example: CVE-2023-29336

In May 2023, Microsoft disclosed a Win32k Elevation of Privilege Vulnerability:

| Detail | Fact |
|--------|------|
| **CVE** | CVE-2023-29336 |
| **Component** | Win32k kernel driver |
| **Affected systems** | Windows Server 2008/2008 R2/2012/2012 R2/2016; Windows 10 |
| **Result of exploitation** | Attacker gains **SYSTEM** privileges — the highest privilege level in Windows |

### Defenses Against Privilege Escalation

| Defense | Description |
|---------|-------------|
| **Patch promptly** | Closes the known vulnerabilities exploited for escalation |
| **Update anti-malware signatures** | Detects known escalation exploit patterns |
| **Data Execution Prevention (DEP)** | Restricts which memory regions can execute code — blocks exploits that run shellcode in data regions |
| **Address Space Layout Randomization (ASLR)** | Randomizes memory addresses each run — makes it harder for attackers to reliably target memory locations |
| **Least privilege** | Users and services should only have the minimum permissions necessary — limits the value of escalation |

---

## Cross-Site Request Forgery (CSRF)

**CSRF** (also written XSRF, pronounced "sea surf") exploits the trust a web server has in an authenticated browser session. If a victim is already logged into a site, the attacker can craft a request that their browser sends on their behalf — and the server, seeing a valid session, executes it.

### How CSRF Works

```
[Victim logs into bank — session established, browser now trusted]
          |
          ▼
[Attacker sends victim a malicious link (email, message, etc.)]
          |
          ▼
[Victim clicks link]
          |
          ▼
[Victim's browser silently sends a funds transfer request to the bank,
 using the victim's authenticated session]
          |
          ▼
[Bank server sees valid session → executes transfer to attacker's account]
          |
          ▼
[Victim is unaware — transaction happens entirely behind the scenes]
```

### Why CSRF Works

Web browsers automatically include session cookies with every request to a domain. When the victim's browser loads the attacker's malicious link, it includes the bank's session cookie — making the forged request appear completely legitimate to the server.

This is sometimes called a **one-click attack** because a single click on a malicious link can trigger a server-side action.

### CSRF vs. XSS

| Property | CSRF | XSS |
|----------|------|-----|
| **Exploits** | Server's trust in the user's browser | User's trust in the server's content |
| **Requires victim to be logged in?** | Yes | Not necessarily |
| **Executes code on** | Server (via forged request) | Victim's browser |

### Defenses Against CSRF

| Defense | Description |
|---------|-------------|
| **Anti-CSRF tokens** | A cryptographic token included with every state-changing request; the server validates it before executing — forged requests lack the correct token |
| **SameSite cookie attribute** | Prevents cookies from being sent with cross-site requests |
| **Verify HTTP Referer header** | Check that requests originate from the expected domain |
| **Require re-authentication for sensitive actions** | Fund transfers, password changes, etc. require the user to re-enter credentials |

---

## Directory Traversal

**Directory traversal** is a web server misconfiguration (or software vulnerability) that allows an attacker to access files outside the intended web root directory.

### What Should Be Accessible

A web server is configured to serve files only from its designated web root (e.g., `C:\inetpub\wwwroot` on Windows):

```
C:\
├── Windows\          ← should NOT be accessible via web
├── Program Files\    ← should NOT be accessible via web
├── Users\            ← should NOT be accessible via web
└── inetpub\
    └── wwwroot\      ← only this directory should be web-accessible
```

### How Directory Traversal Works

The `../` sequence in a file path means "go up one directory." If a web server doesn't sanitize path input, an attacker can use repeated `../` sequences to navigate outside the web root:

**Example malicious request (from a web server log):**
```
GET /show.asp?file=../../windows/system.ini
```

Each `../` moves one directory up. Enough repetitions escape the web root entirely, giving read access to arbitrary files on the server — including system configuration files, user data, or application secrets.

> **Detection tip:** Any web request containing `../` in the URL or parameters is a strong indicator of a directory traversal attempt and should be treated as suspicious.

### Causes

| Cause | Description |
|-------|-------------|
| **Misconfigured web server** | Server not restricted to serve only from the web root |
| **Web server software vulnerability** | A bug in the server software that allows specially crafted paths to escape the root |
| **Application vulnerability** | An application running on the server passes user input to file system operations without sanitization |

### Defenses

| Defense | Description |
|---------|-------------|
| **Input validation** | Strip or reject `../` and other path traversal sequences from all user input |
| **Canonicalize file paths** | Resolve the full path before checking whether it falls within the permitted directory |
| **Web server configuration** | Explicitly restrict the server to serve only from the web root |
| **Least privilege for web server process** | Even if traversal succeeds, a restricted process account limits what files can be accessed |

---

## Application Attack Summary

| Attack | Root Cause | Key Defense |
|--------|-----------|-------------|
| **SQL Injection** | Unsanitized input passed to database queries | Parameterized queries |
| **Buffer Overflow** | No bounds checking on memory writes | Bounds checking; DEP; ASLR |
| **Replay / Pass the Hash** | Captured credentials reused without challenge | Encrypted traffic; salted per-session tokens |
| **Privilege Escalation** | Vulnerability allows gaining higher permissions | Patching; least privilege; DEP; ASLR |
| **CSRF** | Server trusts browser session without request verification | Anti-CSRF tokens; SameSite cookies |
| **Directory Traversal** | Web server serves files outside the web root | Input validation; path canonicalization; server configuration |
| **XSS** | Unsanitized user input rendered as executable script | Input validation; output encoding; Content Security Policy |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Privilege escalation** | Exploits vulnerabilities to gain higher permissions; vertical (to admin) and horizontal (to another user) |
| **CVE-2023-29336** | Win32k kernel driver flaw granting SYSTEM privileges on Windows |
| **CSRF** | Forges server requests using the victim's authenticated browser session — one click can execute an action |
| **Anti-CSRF token** | Cryptographic token that proves a request originated from the legitimate user, not a forged link |
| **Directory traversal** | `../` sequences in paths navigate outside the web root to access restricted files |
| **Defense theme** | Input validation, patching, least privilege, and cryptographic verification address the majority of application attack surfaces |
