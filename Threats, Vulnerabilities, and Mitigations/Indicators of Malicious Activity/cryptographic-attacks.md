# Cryptographic Attacks

## Overview

Strong cryptographic algorithms, when correctly implemented, are computationally infeasible to break directly. Attackers know this — so rather than attacking the math, they attack the **weaknesses in implementation** or look for **flaws in the algorithms themselves**. Two significant attack categories target these gaps: **birthday attacks** (targeting hash collisions) and **downgrade attacks** (targeting the negotiation of encryption protocols).

---

## Birthday Attacks & Hash Collisions

### The Birthday Problem

In a room of 23 people, there is approximately a **50% chance** that two of them share a birthday. This seems counterintuitive — but the key insight is that we are not asking whether anyone shares *a specific* birthday. We are asking whether *any two people* share *any* birthday with each other. The number of possible pairs grows rapidly with the number of people.

| People in Room | Probability of a Shared Birthday |
|---------------|----------------------------------|
| 23 | ~50% |
| 30 | ~70% |
| 50 | ~97% |

### Applying This to Cryptography

A **birthday attack** applies this same probability concept to hash functions. The attacker is not looking for a specific hash — they are looking for **any two inputs that produce the same hash output**. This is a **collision**.

```
Input A (plaintext) ──► [Hash Algorithm] ──► Hash: a1b2c3...
Input B (plaintext) ──► [Hash Algorithm] ──► Hash: a1b2c3...  ← collision
```

Finding a collision requires brute-forcing many inputs and comparing their hash outputs — but the probability of finding one grows much faster than intuition suggests, following the same mathematics as the birthday problem.

### Preventing Birthday Attacks

The primary defense is **using a large hash output size**. The larger the hash, the exponentially more inputs must be tried before a collision is likely. This is why:

- **MD5** (128-bit output) — deprecated; collisions found in 1996
- **SHA-1** (160-bit output) — deprecated; practically broken
- **SHA-256** (256-bit output) — current standard; no practical collision attacks known

### Real-World Impact: MD5 Collisions (2008)

| Event | Date |
|-------|------|
| MD5 published | April 1992 |
| Collisions first found by researchers | 1996 |
| Researchers forge a legitimate-looking CA certificate using MD5 collision | December 2008 |

The 2008 attack was the turning point: researchers crafted a digital certificate that appeared to have been signed by a trusted Certificate Authority — but had never actually been signed by one. The MD5 collision allowed them to produce a fraudulent certificate that passed validation. The industry rapidly transitioned away from MD5.

> A hash collision is not just a theoretical concern — the 2008 MD5 attack demonstrated that collisions could be used to forge trusted cryptographic credentials.

---

## Downgrade Attacks

A **downgrade attack** does not break the cryptography directly. Instead, it manipulates the **negotiation process** between two communicating parties to force them to use a **weaker** encryption algorithm — or no encryption at all.

The cryptographic algorithms themselves may be perfectly secure. The vulnerability is in how the implementation allows a weaker option to be selected.

---

## SSL Stripping

**SSL stripping** is a common downgrade attack that combines an **on-path position** with protocol manipulation to strip HTTPS down to HTTP — silently removing encryption from a connection the victim believes is secure.

### Normal Communication (No Attacker)

```
[Browser] ──► HTTPS request ──► [Web Server]
[Browser] ◄── Encrypted response ◄── [Web Server]
```

### SSL Stripping Attack

The attacker sits between the browser and the server. The browser communicates with the attacker over unencrypted HTTP; the attacker communicates with the server over encrypted HTTPS — acting as a proxy that has full visibility into all traffic.

```
[Browser] ──── HTTP (unencrypted) ────► [Attacker] ──── HTTPS (encrypted) ────► [Web Server]
[Browser] ◄─── HTTP (unencrypted) ──── [Attacker] ◄─── HTTPS (encrypted) ◄──── [Web Server]
```

### Step-by-Step SSL Stripping Flow

| Step | Action |
|------|--------|
| **1** | Browser sends initial HTTP GET request (not HTTPS) |
| **2** | Attacker passes the request to the web server as-is |
| **3** | Web server responds with a redirect to HTTPS — attacker **intercepts and discards** this redirect |
| **4** | Attacker independently makes an HTTPS request to the web server, establishing an encrypted session with it |
| **5** | Attacker sends the browser an HTTP OK — the browser never sees the HTTPS redirect |
| **6** | Browser sends login credentials over HTTP (no encryption) — attacker reads them in plaintext |
| **7** | Attacker uses the captured credentials to log into the web server over HTTPS |
| **8** | All subsequent traffic flows in plaintext between browser and attacker; encrypted between attacker and server |

**From the browser's perspective:** The site loaded normally and login succeeded.  
**From the server's perspective:** An authenticated HTTPS session is in progress.  
**In reality:** The attacker has read all credentials and controls the session.

### Why SSL Stripping Works

The attack exploits the initial HTTP request. Most users type a domain name without specifying `https://` — the browser defaults to HTTP, and the server is normally trusted to redirect to HTTPS. The attacker intercepts that redirect before it reaches the browser, leaving the browser permanently on HTTP.

### Defenses Against SSL Stripping

| Defense | Description |
|---------|-------------|
| **HTTP Strict Transport Security (HSTS)** | Server instructs the browser to always use HTTPS for this domain — the browser will refuse to connect over HTTP, even on first visit |
| **HSTS Preloading** | Domain is hardcoded into browsers as HTTPS-only before the first connection ever occurs — eliminates the vulnerable initial HTTP request entirely |
| **Always use HTTPS** | Ensure servers redirect HTTP to HTTPS and that all resources load over HTTPS |
| **VPN** | Encrypts traffic from the device, preventing the attacker from interposing on unencrypted connections |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Birthday attack** | Finds hash collisions by exploiting probability — any two matching hashes, not a specific target |
| **Hash collision** | Two different inputs producing the same hash output; undermines integrity guarantees |
| **MD5** | Deprecated — collisions demonstrated in 1996; CA certificate forged via collision in 2008 |
| **Large hash output** | The primary defense against birthday attacks; SHA-256 (256-bit) is the current standard |
| **Downgrade attack** | Forces negotiation to a weaker algorithm or no encryption — exploits implementation, not the algorithm |
| **SSL stripping** | On-path attack that removes HTTPS by intercepting the initial redirect, leaving the victim on unencrypted HTTP |
| **HSTS** | Browser policy that enforces HTTPS-only connections — prevents the initial HTTP request that enables SSL stripping |
