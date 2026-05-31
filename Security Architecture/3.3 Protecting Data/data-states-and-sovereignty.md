# Data States, Sovereignty & Geolocation

## Overview

Data security requirements change depending on where data is and what is happening to it. The three **states of data** — at rest, in transit, and in use — each present different risks and require different controls. Additionally, where data is physically stored matters legally, and where users are located affects what access they should have.

---

## The Three States of Data

### Data at Rest

**Data at rest** is any data stored on a storage medium — hard drives, SSDs, flash drives, databases, or any other persistent storage — whether or not it is currently being accessed.

| Protection Method | Description |
|------------------|-------------|
| **Full disk encryption** | Encrypts everything on the storage device (BitLocker, FileVault) |
| **Database encryption** | Encrypts specific records or columns within a database |
| **File/folder encryption** | Encrypts individual files or directories using OS tools (EFS) or third-party software |
| **Access controls** | Rights and permissions at the user, group, or role level restrict who can read or modify data |

Even unencrypted data qualifies as data at rest — but unencrypted data at rest is vulnerable to anyone who gains physical or logical access to the storage device.

---

### Data in Transit

**Data in transit** (also called **data in motion**) is data actively moving across a network — through switches, routers, firewalls, and other devices between source and destination.

Unencrypted data in transit is readable by anyone who can tap into the network path.

| Protection Method | Description |
|------------------|-------------|
| **TLS / HTTPS** | Encrypts web traffic between client and server |
| **IPsec / VPN** | Encrypts all traffic across a network tunnel (site-to-site or remote access) |
| **Firewall / IPS** | Enforces traffic policies and blocks known malicious traffic — does not encrypt but filters |
| **Application-layer encryption** | Built into the application itself, independent of network encryption |

---

### Data in Use

**Data in use** is data actively loaded into memory (RAM) or being processed by the CPU. This is the most vulnerable state — data in use is almost always in decrypted, plaintext form because the CPU needs to read it to perform operations.

```
[Data at rest — encrypted on disk]
          |
          ▼ (loaded into RAM)
[Data in use — decrypted in memory] ← most vulnerable state
          |
          ▼ (CPU processes it)
[Results written back to disk — encrypted again]
```

Attackers target data in use specifically because they know encryption does not protect it in this state.

#### Real-World Example: Target (2013)

In November 2013, attackers installed malware on every Target point-of-sale (POS) terminal. The malware captured credit card data from RAM — the moment between a card being swiped and the data being encrypted for storage or transmission.

| State | Target's Protection | Attacker's Result |
|-------|--------------------|--------------------|
| **Data at rest** | Encrypted | Protected |
| **Data in transit** | Encrypted | Protected |
| **Data in use** | Not encrypted (by nature) | **110 million cards stolen** |

Target's encryption was correctly implemented — but the attack targeted the one window where data is necessarily exposed: the moment it exists in memory.

---

## States of Data Comparison

| State | Where Data Is | Encryption Possible? | Primary Threat |
|-------|--------------|---------------------|----------------|
| **At rest** | Stored on disk / database | Yes — full disk, file, or DB encryption | Physical access, database breach |
| **In transit** | Moving across network | Yes — TLS, IPsec, VPN | Network interception, MITM |
| **In use** | In RAM / CPU | Limited — by design | Memory scraping, RAM dumps |

---

## Data Sovereignty

**Data sovereignty** means that data stored within a country's borders is subject to that country's laws — including laws about how the data must be handled, who can access it, and what happens in legal proceedings.

### Why It Matters

- A court order in Country A cannot compel disclosure of data physically stored in Country B (without legal cooperation between countries)
- Some countries require that data about their citizens be stored within their borders
- Regulatory penalties apply under the laws of the country where the data is stored, not necessarily where the organization is headquartered

### GDPR — General Data Protection Regulation

The EU's GDPR is one of the most significant data sovereignty regulations:

| Requirement | Detail |
|-------------|--------|
| **Data residency** | Data collected on EU citizens must be stored within the EU (or countries with equivalent protections) |
| **Right to access** | Individuals can request their data |
| **Right to erasure** | Individuals can request deletion of their data |
| **Breach notification** | Data breaches must be reported to authorities within 72 hours |
| **Scope** | Applies to any organization that handles EU citizens' data — regardless of where the organization is located |

### Practical Implications for Multi-Region Organizations

- Cloud storage region selection matters — storing EU citizen data in a US-region cloud bucket may violate GDPR
- Different offices may operate under different data handling rules
- Legal hold and e-discovery processes must account for which country's laws govern the data

---

## Geolocation

**Geolocation** uses technology to determine a user's physical location. This location can then be used to grant, restrict, or adjust access to data.

### Geolocation Technologies

| Technology | Description |
|-----------|-------------|
| **GPS** | High-accuracy positioning via satellite — common in mobile devices |
| **Wi-Fi (802.11)** | Location estimated from visible access point identifiers |
| **Mobile network** | Cell tower triangulation provides approximate location |
| **IP geolocation** | Approximate country/region based on IP address — less precise |

### Access Control Applications

| Scenario | Geolocation Use |
|----------|----------------|
| **Streaming media** | Content licensed for one country blocked in another |
| **Corporate network access** | Additional permissions granted when physically inside the office |
| **Compliance enforcement** | Access to regulated data restricted to approved geographies |
| **Fraud detection** | Login from unexpected location triggers MFA challenge or alert |
| **Impossible travel detection** | Same account logging in from two distant locations simultaneously flags as suspicious |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Data at rest** | Stored on disk — protect with encryption and access controls |
| **Data in transit** | Moving across a network — protect with TLS, IPsec, or VPN |
| **Data in use** | In RAM or CPU — almost always decrypted; most vulnerable state; primary defense is limiting who can execute code |
| **Target 2013** | Encryption of data at rest and in transit was insufficient — attackers targeted RAM directly, stealing 110 million cards |
| **Data sovereignty** | Data stored in a country is governed by that country's laws |
| **GDPR** | EU regulation requiring EU citizen data to be stored within the EU; applies globally to any organization handling EU data |
| **Geolocation** | Physical location used to control data access — streaming restrictions, impossible travel detection, location-based permissions |
