# Port Security & 802.1X Authentication

## Overview

**Port security** controls which devices and users can access a network at the physical connection level — whether through a wired switch port or a wireless access point. Before any network access is granted, the connecting device must authenticate. This prevents unauthorized devices from simply plugging into an available port and gaining access to internal resources.

The protocol framework that makes this possible is **EAP (Extensible Authentication Protocol)**, standardized for network access control through **IEEE 802.1X**.

---

## EAP — Extensible Authentication Protocol

**EAP** is a flexible authentication framework that can be applied to many types of networks — wired, wireless, VPN, and others. It is not a single authentication method but a framework that supports many different authentication mechanisms.

Because EAP is a framework, manufacturers of wireless access points and wired switches can implement it consistently, allowing all of these devices to integrate with the same backend authentication infrastructure.

---

## 802.1X — Port-Based Network Access Control

**802.1X** is the IEEE standard that applies EAP to network port access. It is sometimes called **NAC (Network Access Control)** or **Port-Based NAC**.

### What It Does

When a device connects to a switch port or wireless access point configured for 802.1X:

- The port is in a **blocked state** — no network access until authentication succeeds
- The device must authenticate using EAP credentials
- Only after successful authentication does the port open and grant network access

---

## The Three Components

802.1X authentication involves three distinct roles:

| Component | Role | Examples |
|-----------|------|---------|
| **Supplicant** | The client device attempting to access the network | Laptop, workstation, smartphone |
| **Authenticator** | The network device controlling access to the port | Switch, wireless access point |
| **Authentication Server** | The backend database that validates credentials | RADIUS, TACACS+, LDAP, Active Directory (via Kerberos) |

---

## Authentication Flow

```
[Supplicant — device connecting]
[Authenticator — switch or AP]
[Authentication Server — RADIUS / AD]

Step 1: Supplicant connects to switch port
          → Port is blocked — no network access

Step 2: Authenticator sends EAP Request
          → "Identify yourself"

Step 3: Supplicant sends EAP Response
          → "I am [device/username]"

Step 4: Authenticator forwards identity to Authentication Server
          → "This device claims to be [device/username]"

Step 5: Authentication Server requests proof
          → "Provide credentials"

Step 6: Authenticator relays request to Supplicant
          → "Provide your credentials"

Step 7: Supplicant provides credentials (password, certificate, etc.)
          → Sent to Authenticator → forwarded to Authentication Server

Step 8: Authentication Server validates credentials
          → Match found → sends "Access Accept" to Authenticator

Step 9: Authenticator opens the port
          → Supplicant now has full network access
```

**If credentials are invalid:**
- Authentication Server sends "Access Reject"
- Authenticator keeps the port blocked
- Supplicant cannot access the network

---

## Authentication Server Options

802.1X works with a variety of backend authentication systems:

| Protocol / System | Description |
|------------------|-------------|
| **RADIUS** | Remote Authentication Dial-In User Service — most common for 802.1X |
| **TACACS+** | Terminal Access Controller Access-Control System Plus — common in Cisco environments |
| **LDAP** | Lightweight Directory Access Protocol — used to query directory services |
| **Active Directory** | Microsoft's directory service; commonly accessed via Kerberos or LDAP |
| **Kerberos** | Ticket-based authentication protocol used in Windows Active Directory environments |

---

## Wired vs. Wireless Port Security

| Property | Wired (Switch Port) | Wireless (Access Point) |
|----------|-------------------|------------------------|
| **Physical connection** | Ethernet cable plugged into switch port | Wi-Fi association |
| **802.1X support** | Yes — per-port authentication | Yes — per-association authentication |
| **Common use** | Enterprise offices, data centers | Corporate Wi-Fi, guest network segmentation |
| **Without 802.1X** | Anyone who plugs in gets network access | Anyone who knows the WPA key gets access |
| **With 802.1X** | Must authenticate with individual credentials | Must authenticate with individual credentials |

---

## Why 802.1X Matters

### Without 802.1X

An attacker who gains physical access to a building can plug into any available Ethernet port and potentially access the internal network. Shared Wi-Fi passwords offer no individual accountability.

### With 802.1X

- Each user or device must authenticate individually before gaining network access
- A stolen device or shared password is insufficient — credentials are tied to individual accounts
- Failed authentication attempts are logged — brute force attempts are visible
- Devices that fail posture assessment can be placed in a restricted VLAN (quarantine)

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Port security** | Controls network access at the physical port level — wired or wireless |
| **EAP** | Flexible authentication framework that supports many methods and integrates with diverse network hardware |
| **802.1X** | IEEE standard applying EAP to port-based access control — port is blocked until authentication succeeds |
| **Supplicant** | The client device requesting network access |
| **Authenticator** | The switch or AP that enforces the authentication requirement |
| **Authentication server** | Backend system (RADIUS, AD, LDAP) that validates credentials |
| **RADIUS** | Most common authentication server protocol used with 802.1X |
| **Without 802.1X** | Physical access to a port = network access — no credential required |
| **With 802.1X** | Physical access alone is insufficient — credentials must be validated before the port opens |
