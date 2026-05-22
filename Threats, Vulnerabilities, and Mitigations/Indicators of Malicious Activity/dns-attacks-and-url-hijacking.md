# DNS Attacks & URL Hijacking

## Overview

The **Domain Name System (DNS)** translates human-readable domain names into IP addresses. It is a foundational service that every internet connection depends on. If an attacker can manipulate DNS — at any point in the resolution chain — they can silently redirect users to malicious destinations without the user ever knowing they went to the wrong place.

---

## DNS Poisoning

**DNS poisoning** (also called DNS spoofing) involves inserting false records into the DNS resolution process so that users are directed to attacker-controlled IP addresses instead of legitimate ones.

### Attack Vectors

| Method | Description | Difficulty |
|--------|-------------|-----------|
| **Modify the DNS server** | Gain access to and alter records on the DNS server itself | High — servers are well-protected |
| **Modify the client hosts file** | Edit the local hosts file on the victim's machine; takes precedence over DNS queries | Medium — requires local access and elevated privileges |
| **Man-in-the-middle interception** | Sit between the client and DNS server; intercept queries and reply with false answers in real time | Medium — requires network position |
| **Domain registrar compromise** | Gain access to the domain's registration account and change DNS settings at the source | Medium — social engineering, credential theft, or brute force |

### How the Hosts File Works

Every operating system has a local **hosts file** that maps domain names to IP addresses. The OS checks this file **before** querying a DNS server. If an entry exists in the hosts file for a domain, that IP is used — regardless of what the DNS server would return.

An attacker who can write to the hosts file on a client machine can redirect any domain silently.

---

## DNS Server Poisoning: Step-by-Step

### Normal Resolution

```
[User 1]
    |
    | (query: what is the IP for professormesser.com?)
    ▼
[DNS Server — legitimate record: professormesser.com → x.x.x.164]
    |
    | (correct IP returned)
    ▼
[User 1 connects to legitimate site]
```

### After DNS Poisoning

```
[Attacker gains access to DNS server]
    |
    | (modifies record: professormesser.com → 100.100.100.100)
    ▼
[User 2 queries DNS server]
    |
    | (poisoned record returned: 100.100.100.100)
    ▼
[User 2 connects to attacker's server — not the legitimate site]
```

Any user who queries the DNS server after the record is changed will be redirected to the attacker's IP — transparently and without any indication that anything is wrong.

---

## Domain Registrar Compromise

If an attacker gains access to a domain's **registrar account** — where DNS settings are managed — they can change DNS records for every service associated with that domain, affecting all users worldwide.

### Common Methods to Access a Registrar Account

- Brute force attack against the registrar login
- Social engineering the registrar's support staff
- Credential stuffing using previously leaked username/password combinations
- Phishing the domain owner

### Real-World Example: Brazilian Bank (October 2016)

| Detail | Fact |
|--------|------|
| **Date** | Saturday, October 22, 2016 at 1:00 PM |
| **Scope** | 36 domains associated with a major Brazilian bank modified |
| **Targets** | Desktop, mobile, and all other bank-facing systems |
| **Duration** | Attackers had control for approximately 6 hours |
| **Impact** | Attackers became the IP address for the entire bank — collecting usernames, passwords, and financial data from all connecting customers |
| **Bank scale** | Over 5 million customers and approximately $27 billion in assets |

During that 6-hour window, any customer who attempted to connect to the bank — on any device — was connecting to attacker-controlled infrastructure while entering their credentials.

---

## URL Hijacking & Typosquatting

**URL hijacking** (also called **typosquatting** or **brandjacking**) exploits the fact that users sometimes mistype domain names. Attackers register domains that are close to legitimate ones and use them to intercept misdirected traffic.

### Typosquatting Techniques

| Technique | Example (vs. `professormesser.com`) |
|-----------|-------------------------------------|
| **Character substitution** | `professormessor.com` (e → o) |
| **Missing character** | `professormesse.com` (missing final r) |
| **Extra character** | `professormessers.com` (extra s) |
| **Different TLD** | `professormesser.org` (same name, different extension) |
| **Homoglyph** | Using visually similar characters (e.g., `rn` instead of `m`) |

### What Attackers Do With Hijacked URLs

| Goal | Method |
|------|--------|
| **Advertising revenue** | Redirect traffic to ad-heavy pages — views and clicks generate income |
| **Credential theft** | Present a page that looks identical to the legitimate site; capture submitted usernames and passwords |
| **Malware delivery** | Prompt visitors to download software that installs ransomware or botnet agents |
| **Competitive redirect** | Send the legitimate site's misdirected traffic directly to a competitor |
| **Domain sale** | Register a valuable misspelling and sell it to the legitimate domain owner |

---

## Protection

### For Users

- **Do not click links in emails** — type domain names directly into the browser
- **Check the URL carefully** before entering any credentials — look for character substitutions, extra letters, or different TLDs
- **Use bookmarks** for frequently visited sensitive sites (banks, email, etc.)
- **Look for HTTPS and a valid certificate** — phishing and typosquatting sites may lack valid TLS certificates for the intended domain

### For Organizations

| Control | Description |
|---------|-------------|
| **DNSSEC** | Cryptographically signs DNS records, making unauthorized modifications detectable |
| **Registrar account MFA** | Prevents unauthorized access to domain settings even if credentials are compromised |
| **Register common misspellings** | Defensively register typosquatted variants of your own domain and redirect them to the legitimate site |
| **Monitor DNS records** | Alert on any unexpected changes to DNS configuration |
| **Restrict hosts file access** | Limit which accounts can modify the local hosts file on managed endpoints |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **DNS poisoning** | False DNS records redirect users to attacker-controlled IP addresses |
| **Hosts file attack** | Local overrides to DNS — checked before any server query; requires local access |
| **DNS server compromise** | Modifying records directly on the server affects all downstream users |
| **Registrar compromise** | Attacker controls DNS at the source — entire domains can be redirected globally |
| **Brazilian bank (2016)** | 36 domains hijacked for 6 hours; all customer traffic redirected to attacker infrastructure |
| **Typosquatting** | Registering misspelled or variant domains to intercept misdirected traffic |
| **Defense** | DNSSEC, registrar MFA, defensive domain registration, URL verification habits |
