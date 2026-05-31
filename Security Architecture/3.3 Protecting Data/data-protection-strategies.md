# Data Protection Strategies

## Overview

Protecting data requires a layered approach — no single technique is sufficient on its own. This page covers the key strategies used to protect data: geographic restrictions, encryption, hashing, obfuscation, tokenization, segmentation, and permission controls. Several of these topics have dedicated detailed docs linked throughout.

---

## Geographic Restrictions & Geofencing

**Geographic restrictions** limit access to data based on where the user is physically located. **Geofencing** defines a virtual boundary — users inside the boundary have one level of access, users outside have a different (typically more restricted) level.

### Determining Location

| Method | Accuracy | Best For |
|--------|----------|---------|
| **IP subnet** | Moderate — accurate for fixed internal networks | Wired corporate systems |
| **GPS** | High | Mobile devices |
| **Wi-Fi (802.11 SSID matching)** | High — cross-references known SSID databases | Mobile devices without GPS |
| **Mobile network (cell towers)** | Moderate | Mobile devices |

IP addresses are unreliable for mobile devices — a phone on a cellular connection could be anywhere. GPS or Wi-Fi-based geolocation is more appropriate for those scenarios.

### Geofencing in Practice

| Scenario | Geofencing Rule |
|----------|----------------|
| **Sensitive data access** | Only allow access when user is physically inside a corporate facility |
| **Compliance enforcement** | Block access to regulated data from outside approved geographic regions |
| **Streaming media** | Content licensed for one country restricted from others |
| **Fraud prevention** | Trigger additional authentication if login location is unusual |

---

## Encryption

**Encryption** converts plaintext into ciphertext — unreadable without the decryption key. It is the most direct method for protecting data confidentiality.

| Term | Definition |
|------|-----------|
| **Plaintext** | The original, readable data |
| **Ciphertext** | The encrypted, unreadable result |
| **Confusion** | The dramatic difference between plaintext and its ciphertext — a desired property of strong encryption |

> **See also:** [Encryption Concepts](encryption-concepts.md) · [Public Key Infrastructure](public-key-infrastructure.md)

---

## Hashing

**Hashing** converts data into a fixed-length fingerprint (message digest). It is a one-way process — the original data cannot be reconstructed from the hash alone.

### Key Uses

| Use Case | How Hashing Helps |
|----------|------------------|
| **Password storage** | Store hash instead of plaintext — even if the database is stolen, passwords are not immediately readable |
| **File integrity** | Compare hash of downloaded file against published hash — if they match, the file is unmodified |
| **Digital signatures** | Hash of a message signed with a private key — proves authenticity and integrity |

**SHA-256** is the current standard — produces 64 hexadecimal characters (256 bits). A single character change in input produces a completely different hash output.

**Collision:** When two different inputs produce the same hash — a critical flaw that caused MD5 to be deprecated.

> **See also:** [Hashing & Digital Signatures](hashing-and-digital-signatures.md)

---

## Obfuscation

**Obfuscation** makes data or code difficult for humans to understand without changing its function. Unlike encryption, it does not provide cryptographic protection — the original can be recovered if the obfuscation method is known.

### Types of Obfuscation

| Type | Description | Example |
|------|-------------|---------|
| **Code obfuscation** | Transforms readable source code into functionally identical but unreadable code | Protecting proprietary code while still distributing it |
| **Data masking** | Hides portions of sensitive values, typically replacing them with placeholders | Credit card receipts showing `**** **** **** 1234` |
| **Steganography** | Hides data within another file (image, audio, video) — covered separately | Secret message embedded in an image |

**Attackers also use obfuscation** — to hide malicious code in scripts or binaries, making it harder for analysts and antivirus tools to detect what the code does.

> **See also:** [Obfuscation](obfuscation.md)

---

## Tokenization

**Tokenization** replaces sensitive data with a non-sensitive substitute (a token) that has no mathematical relationship to the original. Unlike encryption, there is no algorithm to reverse — only a secure lookup table on the token service server.

### Credit Card Tokenization Flow

```
[User registers card on mobile device]
          |
          ▼
[Token Service Server — stores real card, issues tokens]
          |
          ▼
[Phone receives a set of one-time-use tokens]
          |
  [Checkout at store — phone sends token via NFC]
          |
          ▼
[Store sends token to Token Service Server]
          |
          ▼
[Server validates token → confirms real card → approves transaction]
          |
          ▼
[Token is discarded — cannot be reused]
```

**Key property:** Even if an attacker captures the token in transit, it is already expired — it can never be replayed.

> **See also:** [Obfuscation](obfuscation.md) (tokenization section)

---

## Data Segmentation

**Data segmentation** splits data across multiple separate databases or storage systems rather than keeping it all in one place. This limits the damage from any single breach.

### Why Segmentation Helps

| Scenario | Single Database | Segmented Databases |
|----------|----------------|---------------------|
| Attacker breaches the system | All data exposed at once | Only one segment's data exposed |
| Security level | One set of controls for everything | Different controls per data type |
| Regulatory compliance | All data subject to highest-level requirements | Apply controls proportionally |

### Segmentation by Sensitivity

- A database containing only names → minimal security overhead
- A database containing health records or financial data → additional encryption, stricter access controls, audit logging

Segmentation also supports the principle of least privilege at the data tier — services and users only need access to the specific segment relevant to their function.

---

## Permission Restrictions

Access controls ensure that authenticated users can only reach the data they are authorized to access.

### Layers of Permission Control

```
[Authentication]
  - Password policy (minimum length, complexity)
  - Multi-factor authentication
  - Account lockout after failed attempts
          |
          ▼
[Authorization]
  - Role-based access control (RBAC)
  - Group membership
  - File and directory permissions
          |
          ▼
[Data-level controls]
  - Column-level database access
  - Row-level security
  - Field-level encryption for sensitive values
```

**Best practice:** Combine strong authentication with least-privilege authorization. Even a legitimately authenticated user should only see the data required for their specific role.

---

## Data Protection Strategy Summary

| Strategy | What It Protects Against | Key Limitation |
|----------|-------------------------|----------------|
| **Geographic restrictions / Geofencing** | Unauthorized access from wrong location | Location can be spoofed with VPN |
| **Encryption** | Data exposure if storage or network is compromised | Data is decrypted in use — RAM is vulnerable |
| **Hashing** | Password database exposure; file tampering | One-way — cannot recover original (which is the point for passwords) |
| **Obfuscation / Masking** | Casual exposure of sensitive values | Not cryptographically strong — security through obscurity |
| **Tokenization** | Interception of payment data in transit | Requires token service server infrastructure |
| **Data segmentation** | Full database exposure in a breach | Adds architectural complexity |
| **Permission restrictions** | Unauthorized access by authenticated users | Only as strong as the authentication system |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Geofencing** | Combines geolocation with access policy — different permissions based on physical location |
| **Encryption confusion** | Strong encryption produces ciphertext that bears no readable resemblance to plaintext |
| **Hashing** | One-way fingerprint — perfect for passwords and integrity checks; collisions invalidate an algorithm |
| **Code obfuscation** | Makes code unreadable without changing function — used to protect IP and by attackers to hide malware |
| **Data masking** | Hides sensitive values (e.g., last 4 digits of card) — not cryptographically secure |
| **Tokenization** | Replaces sensitive data with a one-time-use token — no mathematical relationship to original |
| **Data segmentation** | Splits data across multiple systems — limits breach scope and allows proportional security controls |
| **Permission restrictions** | Authentication + authorization — users see only what their role requires |
