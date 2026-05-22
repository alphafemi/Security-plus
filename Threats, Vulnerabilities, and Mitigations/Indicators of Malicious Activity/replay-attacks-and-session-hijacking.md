# Replay Attacks & Session Hijacking

## Overview

Not every attack requires breaking encryption or guessing a password. **Replay attacks** work by capturing legitimate credentials or session data and reusing it — presenting stolen proof of identity to a server that has no way to distinguish the attacker from the original user. **Session hijacking** extends this concept by stealing active session tokens that grant access without requiring credentials at all.

---

## Replay Attacks

A **replay attack** captures valid authentication data in transit and retransmits it to a server, impersonating the original user.

### How Attackers Obtain Replay Material

| Method | Description |
|--------|-------------|
| **Physical network tap** | Hardware device inserted into a network cable to copy all passing traffic |
| **ARP poisoning** | Redirects traffic through the attacker's machine (see: On-Path Attacks) |
| **Malware on victim's device** | Captures traffic or credentials locally before they leave the machine |

Once captured, the data is replayed to the server — which sees a valid, authenticated request and grants access.

> **Note:** The replay itself is not an on-path attack, but on-path techniques are commonly used to gather the material for a subsequent replay.

---

## Pass the Hash

**Pass the Hash (PtH)** is a specific replay attack that targets the password hash used during authentication — eliminating the need to ever know the actual password.

### How It Works

```
[Victim] ──► username + hashed password ──► [Server]
               ↑
          [Attacker captures a copy]
               |
               ▼
[Attacker replays username + hashed password to Server]
               |
               ▼
[Server validates hash → grants access to attacker]
```

The server cannot distinguish between the legitimate hash and the replayed hash — both are cryptographically identical. The attacker gains full access without ever knowing the underlying password.

### Defenses Against Replay / Pass the Hash

| Defense | How It Helps |
|---------|-------------|
| **Encrypt all traffic** | Prevents the attacker from reading captured traffic — no hash to steal |
| **Salted hashes with per-session salt** | Each authentication uses a different salt, producing a different hash — a replayed hash from a previous session will not match |
| **Server-side hash uniqueness checks** | Server refuses to accept the same hash twice in a row — a replayed hash is rejected |
| **Kerberos / modern authentication protocols** | Use time-limited tickets and mutual authentication, making raw hash replay far harder |

---

## Session Hijacking (Sidejacking)

When a user logs into a web server, the server creates a **session ID** — a token that identifies the authenticated session. Subsequent requests from that session use the session ID instead of re-entering credentials.

If an attacker captures the session ID, they can use it to make requests to the server that appear to come from the authenticated user — **without ever needing the username or password**.

### Session Hijacking Flow

```
[Victim authenticates with username + password]
          |
          ▼
[Web server creates session ID and sends it to victim]
          |
          ├──► [Victim stores session ID in browser cookie]
          |
          └──► [Attacker captures session ID]
                    |
                    ▼
[Attacker sends requests with stolen session ID]
          |
          ▼
[Web server sees valid session ID → grants attacker full access]
```

The server has no way to know the request is not coming from the original authenticated user — the session ID is the proof of identity.

### How Session IDs Are Stolen

| Method | Description |
|--------|-------------|
| **Packet capture** | Tools like Wireshark or Kismet capture unencrypted HTTP headers containing session cookies |
| **Cross-site scripting (XSS)** | Malicious scripts extract cookie data from the victim's browser and transmit it to the attacker |
| **Browser cookie theft** | Malware or browser extensions read stored cookies directly |
| **Header manipulation** | Tools like Firesheep, Tamper, and Scapy intercept or modify HTTP headers |

### What's in a Cookie?

Browser cookies are not executable — they contain no malicious code. But they do hold:

- **Session IDs** — the primary target for hijacking
- **Usernames and account identifiers**
- **Tracking and preference data**
- **Connection details** that reveal browsing history and behavior

---

## Defenses Against Session Hijacking

| Defense | Description |
|---------|-------------|
| **HTTPS everywhere** | Encrypts the entire communication including headers and cookies — prevents packet capture of session IDs |
| **HTTPS enforcement extensions** | Browser extensions that force HTTPS connections and block plain HTTP fallback |
| **VPN** | Encrypts traffic from the device to the VPN concentrator — protects against local network interception |
| **Session ID expiration** | Short-lived session tokens reduce the window of opportunity for reuse |
| **HttpOnly cookies** | Prevents JavaScript (including XSS scripts) from reading session cookies |
| **Secure cookie flag** | Ensures cookies are only transmitted over HTTPS connections — never over plain HTTP |
| **Session invalidation on logout** | Ensures session IDs are permanently invalidated when a user logs out |

---

## Replay vs. Session Hijacking

| Property | Replay Attack (PtH) | Session Hijacking |
|----------|--------------------|--------------------|
| **What is stolen** | Password hash | Session ID / cookie |
| **When it is used** | To authenticate (login) | To access an already-authenticated session |
| **Credentials required?** | No — hash is sufficient | No — session ID is sufficient |
| **Primary target** | Authentication systems | Web applications |
| **Key defense** | Salted hashes; encrypted traffic | HTTPS; HttpOnly/Secure cookies; session expiration |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Replay attack** | Captured authentication data is retransmitted to gain unauthorized access |
| **Pass the Hash** | Replays a captured password hash — no knowledge of the actual password needed |
| **Session ID** | Token that authenticates a browser session; stealing it grants access without credentials |
| **Session hijacking** | Attacker uses a stolen session ID to impersonate an authenticated user |
| **Cookie** | Not executable, but contains session IDs and other data valuable for hijacking |
| **Primary defense** | End-to-end encryption (HTTPS) prevents session IDs and hashes from being captured in transit |
