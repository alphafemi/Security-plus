# Wireless Authentication & AAA

## Overview

Securing a wireless network requires more than encrypting data in transit — it requires controlling who can connect in the first place. This page covers wireless encryption protocols (WPA2 vs. WPA3), authentication methods (pre-shared key vs. enterprise), and the AAA framework (Authentication, Authorization, Accounting) that underlies centralized network authentication.

---

## WPA2 Security Concerns

**WPA2** has been the wireless encryption standard for many years but has a significant vulnerability in its initial connection process.

### The Four-Way Handshake Attack

WPA2 uses a **four-way handshake** during the initial connection between a device and an access point. This handshake produces a hash of the pre-shared key (PSK) that is transmitted across the network.

**Attack process:**
1. Attacker captures the WPA2 handshake (or forces a reconnection to capture it)
2. The hash of the PSK is extracted from the captured handshake
3. Attacker takes the hash **offline** and runs brute-force attacks against it
4. Modern GPU processing and cloud-based cracking can recover the PSK in days

Once the PSK is recovered, the attacker can connect to the network — and because everyone shares the same PSK, the attacker can also decrypt other users' traffic.

---

## WPA3 Improvements

**WPA3** addresses WPA2's handshake vulnerability with fundamental changes to the authentication and key exchange process.

### Key WPA3 Features

| Feature | Description |
|---------|-------------|
| **GCMP** (Galois Counter Mode Protocol) | Stronger block cipher mode replacing WPA2's CCMP — provides both encryption and integrity |
| **Galois Message Authentication Code (GMAC)** | Message integrity check built into GCMP |
| **SAE** (Simultaneous Authentication of Equals) | New handshake protocol — replaces the four-way handshake |
| **Dragonfly handshake** | The IEEE standard name for SAE |
| **Per-user session keys** | Each user gets a unique session key even when sharing the same PSK |
| **Mutual authentication** | Both the client and the AP authenticate each other |

### Why WPA3 Eliminates the Brute Force Risk

```
WPA2:
[Device] ── four-way handshake (PSK hash transmitted) ──► [AP]
              ↑ attacker captures this hash → brute force offline

WPA3 (SAE):
[Device] ── SAE exchange (session keys derived locally) ──► [AP]
              No hash sent across the network → nothing to brute force
```

Session keys are derived **on each device independently** using a Diffie-Hellman-based exchange — the key never crosses the network. Even if all users share the same PSK, each session uses a different key, so one user cannot decrypt another's traffic.

---

## Wireless Authentication Modes

### Open (No Security)

No authentication or encryption. Any device can connect and read all traffic. Appropriate only for intentionally public, unsecured hotspots.

### WPA3-Personal (PSK)

All users share the same pre-shared key (passphrase). Common in home networks.

- Simple to set up — one password for all users
- SAE prevents offline brute force of the PSK
- **Limitation:** If the PSK is shared or compromised, all users are affected

### WPA3-Enterprise (802.1X)

Each user authenticates with individual credentials (username + password + optional MFA). Authentication is handled by a centralized server.

- Individual accountability — each user has their own credentials
- Compromised credentials affect only that user
- Centralized management — disable one account to revoke access immediately
- Required for corporate environments

| Mode | Authentication | Use Case |
|------|---------------|---------|
| **Open** | None | Public hotspots |
| **WPA3-Personal (PSK)** | Shared passphrase | Home networks |
| **WPA3-Enterprise (802.1X)** | Individual credentials via RADIUS/LDAP | Corporate networks |

---

## AAA Framework

**AAA** stands for **Authentication, Authorization, and Accounting** — the three components of a complete network access control system.

| Component | Definition | Example |
|-----------|-----------|---------|
| **Authentication** | Verifying identity — who are you? | Username + password validated against a directory |
| **Authorization** | Determining access — what are you allowed to do? | Which network resources this user can reach |
| **Accounting** | Recording activity — what did you do? | Login time, data transferred, logout time |

A fourth component often referenced alongside AAA is **Identification** — the initial claim of identity (e.g., providing a username) before authentication occurs.

### AAA Flow

```
[User provides username] ← Identification
          |
          ▼
[User provides password] ← Authentication
          |
          ▼
[Server grants appropriate access level] ← Authorization
          |
          ▼
[Session activity logged] ← Accounting
```

---

## RADIUS

**RADIUS** (Remote Authentication Dial-In User Service) is the most widely used AAA server protocol. Despite "dial-in" in the name, RADIUS is used for all types of network access — wireless, wired, VPN, and remote management.

### What RADIUS Does

- Accepts authentication requests from network devices (switches, APs, VPN concentrators)
- Validates credentials against a user database (Active Directory, LDAP, local accounts)
- Returns an Access-Accept or Access-Reject response
- Logs accounting data for each session

### Why RADIUS Is Widely Used

- Supported by virtually all network devices
- Centralized credential management — one server for all devices
- Single point for access revocation — disable an account and it applies everywhere

---

## 802.1X and EAP

**802.1X** (Network Access Control) is the IEEE standard that enforces authentication before network access is granted. **EAP** (Extensible Authentication Protocol) is the framework that carries the authentication exchange within 802.1X.

> For detailed 802.1X authentication flow and component descriptions, see: [Port Security & 802.1X](port-security-and-802.1x.md)

### Three Components

| Role | Description |
|------|-------------|
| **Supplicant** | The device/user trying to connect |
| **Authenticator** | The network device enforcing authentication (AP or switch) |
| **Authentication Server** | AAA server (RADIUS, LDAP, TACACS+) that validates credentials |

### EAP Flexibility

EAP is a **framework** — not a single method. It supports many different authentication mechanisms:

| EAP Method | Characteristic |
|-----------|---------------|
| **EAP-TLS** | Certificate-based — most secure |
| **PEAP** | Protected EAP — wraps inner authentication in TLS |
| **EAP-TTLS** | Tunneled TLS — similar to PEAP |

Manufacturers can also create proprietary EAP methods for specific device requirements.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **WPA2 four-way handshake** | Sends a hash of the PSK across the network — can be captured and brute-forced offline |
| **WPA3 SAE** | Derives session keys locally — no hash transmitted; eliminates offline brute force |
| **GCMP** | WPA3's encryption protocol — stronger than WPA2's CCMP; includes built-in integrity check |
| **Per-user session keys** | Even on a shared PSK network, each WPA3 user has a unique session key |
| **WPA3-Personal** | Pre-shared key with SAE — for home networks |
| **WPA3-Enterprise** | Individual credentials via 802.1X — required for corporate environments |
| **AAA** | Authentication (who?), Authorization (what access?), Accounting (what did they do?) |
| **RADIUS** | Most common AAA server — centralizes authentication for all network devices |
| **802.1X / EAP** | Port-based network access control; EAP carries the authentication within the 802.1X framework |
