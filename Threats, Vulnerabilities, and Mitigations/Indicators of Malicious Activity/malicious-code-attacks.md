# Malicious Code Attacks

## Overview

When social engineering, default credentials, and misconfigurations have all been addressed, attackers turn to **malicious code** — technical exploits that require skill to develop but can execute automatically once deployed. Unlike walking through an unlocked door, malicious code attacks pick the lock.

Malicious code is a broad category. The delivery mechanism, payload, and target all vary — which means defenses must be equally broad.

---

## Forms of Malicious Code

| Type | Description |
|------|-------------|
| **Executables** | Compiled programs that run directly on the target system |
| **Scripts** | Interpreted code (PowerShell, JavaScript, Python) that runs within an existing environment |
| **Macro viruses** | Code embedded in documents (e.g., Microsoft Office macros) that executes when the file is opened |
| **Trojan horses** | Software disguised as legitimate applications that installs malicious code when run |
| **Worms** | Self-propagating code that spreads automatically across networks |
| **Ransomware** | Encrypts files and demands payment for the decryption key |
| **Web-injected scripts** | Malicious JavaScript or other code injected into legitimate websites |
| **SQL injection payloads** | Code injected into database queries to extract or manipulate data |

---

## Defense in Depth Against Malicious Code

Because malicious code takes so many forms, no single defense is sufficient. A layered approach is required:

| Layer | Protects Against |
|-------|-----------------|
| **Anti-malware software** | Executables, scripts, macro viruses, Trojans — known and behavioral threats |
| **Firewall** | Blocks known malicious traffic at the network perimeter |
| **Continuous patching** | Closes the vulnerabilities malicious code exploits to gain initial access |
| **User security training** | Prevents social engineering and unsafe habits that deliver malicious code (clicking links, opening attachments) |
| **Input validation (applications)** | Prevents injection attacks (SQL, XSS) from executing malicious code via user input |
| **Web Application Firewall (WAF)** | Filters malicious requests to web applications before they reach the server |

---

## Real-World Examples

### WannaCry Ransomware — SMB Exploit (2017)

WannaCry combined a network worm with ransomware, exploiting a critical vulnerability in Windows networking.

| Detail | Fact |
|--------|------|
| **Vulnerability** | SMB (Server Message Block) version 1 — a Windows file-sharing protocol |
| **Exploit used** | EternalBlue — allowed arbitrary code execution on unpatched Windows systems |
| **Attack chain** | Exploit SMB vulnerability → gain OS access → install ransomware |
| **Propagation** | Self-spreading worm — infected every reachable unpatched Windows machine automatically |
| **Impact** | Hundreds of thousands of systems across 150+ countries; hospitals, telecoms, government agencies |
| **Prevention** | Patch MS17-010 (available before the attack); disable SMBv1 |

```
[Unpatched Windows system]
          |
          ▼ (EternalBlue exploits SMBv1)
[Arbitrary code execution on OS]
          |
          ▼
[Ransomware installed — all files encrypted]
          |
          ▼
[System scans network for other unpatched machines → repeats]
```

---

### British Airways — Cross-Site Scripting (2018)

A sophisticated XSS supply chain attack targeted customers at the point of payment.

| Detail | Fact |
|--------|------|
| **Target** | British Airways checkout pages (flight purchase flow) |
| **Attack type** | Cross-site scripting (XSS) via malicious JavaScript injection |
| **Code volume** | 22 lines of JavaScript — minimal footprint, maximum impact |
| **Data stolen** | Credit card numbers, names, billing addresses entered during checkout |
| **Victims** | Approximately 380,000 customers potentially affected |
| **Duration** | Active for weeks before discovery |

The attackers gained access to the British Airways website and inserted 22 lines of JavaScript into the payment pages. Every customer who completed a purchase during that period had their payment details silently exfiltrated to an attacker-controlled server.

> 22 lines of code affecting 380,000 victims illustrates how disproportionate the impact of a well-placed injection can be.

---

### Estonian Central Health Database — SQL Injection

A SQL injection attack against a national healthcare system exposed the medical records of an entire country's population.

| Detail | Fact |
|--------|------|
| **Target** | Estonian Central Health Database — a national healthcare registry |
| **Attack type** | SQL injection |
| **Impact** | Full access to the entire database |
| **Data exposed** | Health information for all citizens of Estonia |

Insufficient input validation in the database-facing application allowed the attacker to inject SQL commands, bypassing access controls and gaining direct access to all records.

---

## Attack Comparison

| Attack | Code Type | Entry Point | Data Targeted |
|--------|-----------|------------|---------------|
| **WannaCry** | Worm + ransomware executable | Unpatched SMBv1 | Files on disk (encrypted for ransom) |
| **British Airways** | Injected JavaScript (XSS) | Compromised website | Payment card data entered by customers |
| **Estonian Health DB** | SQL injection payload | Unsanitized application input | National healthcare records |

---

## Key Lessons

| Lesson | Applies To |
|--------|-----------|
| **Patch immediately** | WannaCry exploited a known vulnerability with a patch already available — unpatched systems were the entire attack surface |
| **Validate all input** | SQL injection and XSS both succeed because applications accepted unvalidated user (or attacker) input |
| **Audit third-party code** | The British Airways attack exploited access to the website itself — supply chain and website integrity monitoring matter |
| **Minimize attack surface** | Disabling SMBv1 (unnecessary on most networks) would have blocked WannaCry's propagation entirely |
| **Defense in depth** | No single control stops all malicious code — layered defenses catch what individual tools miss |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Malicious code** | A broad category including executables, scripts, macros, trojans, worms, injections, and more |
| **WannaCry** | SMBv1 exploit + worm propagation + ransomware — patching and disabling legacy protocols would have prevented it |
| **British Airways XSS** | 22 lines of JavaScript silently stole payment details from 380,000 customers |
| **Estonian health DB** | SQL injection gave full access to a national health database |
| **Defense** | Anti-malware, firewalls, patching, input validation, user training — all required in combination |
