# Zero-Day Vulnerabilities & Attacks

## Overview

Every application and operating system in use today almost certainly contains security vulnerabilities — they simply haven't been discovered yet. When a vulnerability is found and exploited **before the vendor is aware of it and before a patch exists**, it is called a **zero-day attack**.

The name refers to the number of days the vendor has had to respond: zero.

---

## The Vulnerability Discovery Race

Vulnerabilities are discovered by two groups with very different intentions:

| Discoverer | Action Taken | Outcome |
|-----------|-------------|---------|
| **Security researcher** | Reports vulnerability to the vendor (responsible disclosure) | Vendor develops and releases a patch |
| **Attacker** | Keeps the vulnerability secret; develops exploit code | Vulnerability is weaponized with no patch available |

Attackers work continuously to find vulnerabilities before researchers and vendors do. If they succeed, they gain an asymmetric advantage: they can exploit the flaw freely while the vendor has no knowledge of the problem and no way to fix it.

---

## How a Zero-Day Attack Unfolds

```
[Vulnerability exists in software — unknown to vendor]
          |
          ▼
[Attacker discovers and documents the vulnerability]
          |
          ▼
[Attacker develops exploit code — vendor still unaware]
          |
          ▼
[Attacker begins exploiting systems in the wild]
          |
          ▼
[Security community detects the new attack pattern]
          |
          ▼
[Vendor is notified — begins emergency patch development]
          |
          ▼
[Patch released — window of exposure closes]
          |
          ▼
[Systems remain vulnerable until the patch is applied]
```

During the window between discovery and patch release, the attacker has unrestricted use of the exploit. There is no signature, no patch, and no direct defense — because from the defender's perspective, the problem doesn't exist yet.

---

## Why Zero-Days Are So Dangerous

- **No patch exists** — conventional patch management cannot address an unknown vulnerability
- **No detection signatures** — security tools cannot identify attacks they have never seen
- **Vendor has no knowledge** — the software developer cannot fix what they don't know is broken
- **Difficult to defend against** — organizations can only rely on defense in depth and behavioral monitoring to detect unusual activity

---

## Real-World Examples (2023)

### Google Chrome — April 2023

A zero-day involving **memory corruption and a sandbox escape** in the Chrome browser was actively exploited in the wild before Google became aware of it. Google released an emergency patch once the vulnerability was identified.

### Microsoft — May 2023

A zero-day allowed **self-signed code to execute during the UEFI boot process**, bypassing Secure Boot protections. Secure Boot is specifically designed to prevent unauthorized code from running at startup — this exploit undermined that guarantee entirely.

### Apple iOS & iPadOS — May 2023

Three separate zero-day vulnerabilities were patched simultaneously, all reported as actively exploited:

| Vulnerability | Type |
|--------------|------|
| Sandbox escape | Bypasses application isolation |
| Sensitive information disclosure | Leaks private data to unauthorized parties |
| Arbitrary code execution | Allows attacker to run any code on the device |

All three were being exploited in the wild at the time of disclosure, making rapid patching critical.

---

## Tracking Zero-Days & Vulnerabilities

The **Common Vulnerabilities and Exposures (CVE)** database is the authoritative public registry of known vulnerabilities. Each entry receives a unique CVE identifier and severity rating.

- **Website:** [cve.mitre.org](https://cve.mitre.org)
- **Use:** Track newly disclosed vulnerabilities, research CVE details for specific software, monitor for zero-days affecting systems in your environment

Subscribing to CVE alerts for software your organization uses is a practical way to stay ahead of emerging threats.

---

## Defense Strategies

Because zero-days are by definition unknown, traditional signature-based defenses cannot stop them. Defense requires layers:

| Strategy | How It Helps |
|----------|-------------|
| **Patch quickly when patches arrive** | Closes the window of exposure as soon as a fix is available |
| **Defense in depth** | Multiple overlapping security layers increase the chance of detecting unusual behavior |
| **Behavioral monitoring** | Detects anomalous activity even without a known signature for the attack |
| **Least privilege** | Limits the damage an exploit can do by restricting what the compromised process can access |
| **Network segmentation** | Contains the blast radius if an exploit succeeds |
| **Monitor CVE feeds** | Provides early warning when vulnerabilities affecting your stack are disclosed |

> The most important action after a zero-day patch is released is **applying it as quickly as possible**. Once a vulnerability is public, attackers who were previously unaware of it will immediately begin developing exploit code — converting a zero-day into a broadly weaponized attack.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Zero-day vulnerability** | A flaw unknown to the vendor — no patch exists |
| **Zero-day attack** | Active exploitation of a zero-day vulnerability before any fix is available |
| **Attacker advantage** | Attackers who find vulnerabilities first can exploit them freely while defenders are blind to the threat |
| **CVE** | Public database of known vulnerabilities at cve.mitre.org — essential for tracking emerging threats |
| **Chrome / Microsoft / Apple (2023)** | High-profile zero-days affecting browsers, boot processes, and mobile OS — all exploited in the wild before patches |
| **Defense** | Patch immediately when available; use behavioral monitoring and defense in depth for unknown threats |
