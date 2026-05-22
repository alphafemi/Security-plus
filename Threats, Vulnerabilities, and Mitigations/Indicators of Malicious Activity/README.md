# Security Documentation

A comprehensive reference covering cryptography, threat intelligence, exploitation techniques, malware, network attacks, and defensive best practices. Based on the CompTIA Security+ curriculum.

---

## Table of Contents

### Cryptography

| Document | Description |
|----------|-------------|
| [Public Key Infrastructure](public-key-infrastructure.md) | PKI concepts, symmetric vs. asymmetric encryption, key escrow, and the Alice & Bob model |
| [Encryption Concepts](encryption-concepts.md) | Data at rest, database encryption, data in transit, encryption algorithms, key length, and key stretching |
| [Key Exchange](key-exchange.md) | Out-of-band vs. in-band key exchange, session keys, and key exchange algorithms |
| [Cryptographic Hardware & Key Management](cryptographic-hardware-and-key-management.md) | TPM, HSM, key management systems, and secure enclaves |
| [Obfuscation](obfuscation.md) | Steganography, tokenization, data masking, and machine identification codes |
| [Hashing & Digital Signatures](hashing-and-digital-signatures.md) | Hash functions, SHA-256, collisions, salted passwords, rainbow tables, and digital signatures |
| [Blockchain](blockchain.md) | Distributed ledgers, blocks, hashing for integrity, and tamper detection |
| [Digital Certificates & Certificate Authorities](digital-certificates-and-certificate-authorities.md) | X.509 certificates, CAs, CSR process, internal CAs, wildcard certs, CRL, OCSP, and OCSP stapling |
| [Cryptographic Attacks](cryptographic-attacks.md) | Birthday attacks, hash collisions, MD5 weaknesses, downgrade attacks, and SSL stripping |

---

### Threats & Threat Actors

| Document | Description |
|----------|-------------|
| [Threat Actors](threat-actors.md) | Nation states, unskilled attackers, hacktivists, insider threats, organized crime, and shadow IT |
| [Threat Vectors](threat-vectors.md) | Messaging, image-based, file-based, voice, USB/physical, software, network, default credentials, and supply chain vectors |
| [Misinformation & Influence Campaigns](misinformation-and-influence-campaigns.md) | Disinformation, social media amplification, fake accounts, and brand impersonation |

---

### Social Engineering

| Document | Description |
|----------|-------------|
| [Phishing](phishing.md) | Email phishing, vishing, smishing, typosquatting, pretexting, and common scam types |
| [Impersonation](impersonation.md) | Persona techniques, information elicitation, identity fraud, and prevention |
| [Watering Hole Attacks](watering-hole-attacks.md) | Third-party site compromise, targeted poisoning, and defense in depth |

---

### Network Attacks

| Document | Description |
|----------|-------------|
| [Denial of Service](denial-of-service.md) | DoS vs. DDoS, botnets, amplification attacks, DNS amplification, and defenses |
| [DNS Attacks & URL Hijacking](dns-attacks-and-url-hijacking.md) | DNS poisoning, hosts file attacks, registrar compromise, typosquatting, and the 2016 Brazilian bank attack |
| [On-Path Attacks](on-path-attacks.md) | ARP poisoning, man-in-the-middle, and on-path browser (man-in-the-browser) attacks |
| [Replay Attacks & Session Hijacking](replay-attacks-and-session-hijacking.md) | Pass the hash, session ID theft, cookie hijacking, and defenses |
| [Wireless Attacks](wireless-attacks.md) | Deauthentication attacks, 802.11 management frames, RF jamming, and the fox hunt technique |

---

### Application & Exploitation Attacks

| Document | Description |
|----------|-------------|
| [SQL Injection](sql-injection.md) | SQL injection mechanics, OR 1=1 technique, impact, and parameterized query defenses |
| [Cross-Site Scripting (XSS)](cross-site-scripting.md) | Reflected vs. stored XSS, session hijacking, Subaru vulnerability (2017), and defenses |
| [Application Attacks](application-attacks.md) | Privilege escalation, CSRF, directory traversal, CVE-2023-29336, and the full attack summary table |
| [Memory Injection](memory-injection.md) | Process injection, DLL injection, privilege escalation via memory, and detection challenges |
| [Buffer Overflow](buffer-overflow.md) | Memory overflow mechanics, privilege escalation via overflow, and mitigations (ASLR, DEP, stack canaries) |
| [Race Conditions](race-conditions.md) | TOCTOU attacks, banking transaction example, Mars rover Spirit, and Tesla Pwn2Own 2023 |
| [Malicious Code Attacks](malicious-code-attacks.md) | WannaCry (SMB), British Airways XSS, Estonian health database SQL injection, and layered defenses |

---

### Malware

| Document | Description |
|----------|-------------|
| [Malware & Ransomware](malware-and-ransomware.md) | Malware types overview, ransomware mechanics, offline backup defense, and patching |
| [Viruses & Worms](viruses-and-worms.md) | Virus types, fileless viruses, WannaCry worm, antivirus signatures, and defenses |
| [Spyware & Bloatware](spyware-and-bloatware.md) | Spyware capabilities, keylogging, bloatware risks, and removal approaches |
| [Keyloggers, Logic Bombs & Rootkits](keyloggers-logic-bombs-rootkits.md) | Keylogger capture scope, logic bomb triggers, South Korea 2013 attack, rootkits, and Secure Boot |

---

### Infrastructure & Platform Security

| Document | Description |
|----------|-------------|
| [OS Vulnerabilities & Patch Management](os-vulnerabilities-and-patch-management.md) | Vulnerability lifecycle, Patch Tuesday, patch best practices, and environment-based guidance |
| [Malicious Updates](malicious-updates.md) | Fake update prompts, trust indicators, SolarWinds Orion (2020) supply chain attack |
| [IoT Security, Firmware & Legacy Systems](iot-firmware-and-legacy-systems.md) | Firmware risks, Trane thermostat vulnerability, EOL vs. EOSL, and legacy system mitigations |
| [Virtual Machine Security](virtual-machine-security.md) | VM escape, resource reuse, Pwn2Own 2017 exploit chain, and hypervisor patching |
| [Cloud Security Vulnerabilities](cloud-security-vulnerabilities.md) | MFA gaps, unpatched cloud code, Log4j, Spring Cloud, directory traversal, and attack chains |
| [Mobile Device Security](mobile-device-security.md) | Jailbreaking, rooting, sideloading, MDM controls, and acceptable use policies |
| [Common Security Misconfigurations](common-security-misconfigurations.md) | Open cloud storage, weak admin credentials, unencrypted protocols, default credentials, and open ports |

---

### Supply Chain & Physical Security

| Document | Description |
|----------|-------------|
| [Supply Chain Security](supply-chain-security.md) | Third-party risk, Target 2013 HVAC breach, counterfeit hardware, SolarWinds 2020, and vendor audits |
| [Physical Attacks](physical-attacks.md) | Brute force entry, RFID cloning, environmental attacks (power, HVAC, fire suppression) |

---

## Quick Reference: Real-World Incidents

| Incident | Year | Type | Document |
|----------|------|------|----------|
| MD5 CA certificate forgery | 2008 | Cryptographic attack | [Cryptographic Attacks](cryptographic-attacks.md) |
| Stuxnet worm | ~2010 | Nation-state APT | [Threat Actors](threat-actors.md) |
| Target HVAC breach | 2013 | Supply chain | [Supply Chain Security](supply-chain-security.md) |
| South Korea bank logic bomb | 2013 | Logic bomb | [Keyloggers, Logic Bombs & Rootkits](keyloggers-logic-bombs-rootkits.md) |
| Trane thermostat vulnerability | 2014–2016 | IoT firmware | [IoT Security, Firmware & Legacy Systems](iot-firmware-and-legacy-systems.md) |
| Subaru XSS + token vulnerability | 2017 | Cross-site scripting | [Cross-Site Scripting](cross-site-scripting.md) |
| WannaCry ransomware | 2017 | Worm + ransomware | [Viruses & Worms](viruses-and-worms.md) |
| Pwn2Own VM escape (VMware) | 2017 | Virtual machine escape | [Virtual Machine Security](virtual-machine-security.md) |
| British Airways XSS | 2018 | Cross-site scripting | [Malicious Code Attacks](malicious-code-attacks.md) |
| SolarWinds Orion | 2020 | Supply chain | [Malicious Updates](malicious-updates.md) · [Supply Chain Security](supply-chain-security.md) |
| Brazilian bank DNS hijack | 2016 | DNS registrar compromise | [DNS Attacks & URL Hijacking](dns-attacks-and-url-hijacking.md) |
| Fake Cisco switches | 2020 | Counterfeit hardware | [Supply Chain Security](supply-chain-security.md) |
| CVE-2023-29336 (Win32k) | 2023 | Privilege escalation | [Application Attacks](application-attacks.md) |
| Tesla Pwn2Own TOCTOU | 2023 | Race condition | [Race Conditions](race-conditions.md) |

---

## Quick Reference: Key Concepts

| Term | Document |
|------|----------|
| AES / DES | [Encryption Concepts](encryption-concepts.md) |
| ASLR / DEP | [Buffer Overflow](buffer-overflow.md) · [Application Attacks](application-attacks.md) |
| Blockchain | [Blockchain](blockchain.md) |
| Certificate Authority (CA) | [Digital Certificates & Certificate Authorities](digital-certificates-and-certificate-authorities.md) |
| CSRF | [Application Attacks](application-attacks.md) |
| CVE database | [Zero-Day Vulnerabilities](zero-day-vulnerabilities.md) |
| Defense in depth | [Watering Hole Attacks](watering-hole-attacks.md) |
| Digital signature | [Hashing & Digital Signatures](hashing-and-digital-signatures.md) |
| DLL injection | [Memory Injection](memory-injection.md) |
| EOSL | [IoT Security, Firmware & Legacy Systems](iot-firmware-and-legacy-systems.md) |
| HSM | [Cryptographic Hardware & Key Management](cryptographic-hardware-and-key-management.md) |
| HSTS | [Cryptographic Attacks](cryptographic-attacks.md) |
| Jailbreaking / Rooting | [Mobile Device Security](mobile-device-security.md) |
| Key exchange algorithm | [Key Exchange](key-exchange.md) |
| MDM | [Mobile Device Security](mobile-device-security.md) |
| OCSP stapling | [Digital Certificates & Certificate Authorities](digital-certificates-and-certificate-authorities.md) |
| Pass the hash | [Replay Attacks & Session Hijacking](replay-attacks-and-session-hijacking.md) |
| Patch Tuesday | [OS Vulnerabilities & Patch Management](os-vulnerabilities-and-patch-management.md) |
| PKI | [Public Key Infrastructure](public-key-infrastructure.md) |
| Ransomware | [Malware & Ransomware](malware-and-ransomware.md) |
| RFID cloning | [Physical Attacks](physical-attacks.md) |
| Salted hash | [Hashing & Digital Signatures](hashing-and-digital-signatures.md) |
| Secure Boot | [Keyloggers, Logic Bombs & Rootkits](keyloggers-logic-bombs-rootkits.md) |
| Secure enclave | [Cryptographic Hardware & Key Management](cryptographic-hardware-and-key-management.md) |
| Session hijacking | [Replay Attacks & Session Hijacking](replay-attacks-and-session-hijacking.md) |
| SQL injection | [SQL Injection](sql-injection.md) |
| Steganography | [Obfuscation](obfuscation.md) |
| TOCTOU | [Race Conditions](race-conditions.md) |
| Tokenization | [Obfuscation](obfuscation.md) |
| TPM | [Cryptographic Hardware & Key Management](cryptographic-hardware-and-key-management.md) |
| Typosquatting | [DNS Attacks & URL Hijacking](dns-attacks-and-url-hijacking.md) · [Phishing](phishing.md) |
| VM escape | [Virtual Machine Security](virtual-machine-security.md) |
| X.509 | [Digital Certificates & Certificate Authorities](digital-certificates-and-certificate-authorities.md) |
| XSS | [Cross-Site Scripting](cross-site-scripting.md) |
| Zero-day | [Zero-Day Vulnerabilities](zero-day-vulnerabilities.md) |
