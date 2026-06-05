# Firewall Rule Configuration & IPS Signatures

## Overview

A firewall is only as effective as its rules. Understanding how rule bases are structured, how to read and write ACL rules, and how IPS signatures work is essential for configuring and maintaining network security controls.

> **See also:** [Firewalls](firewalls.md) for an overview of firewall types (traditional, UTM, NGFW, WAF) · [IDS & IPS](ids-and-ips.md) for inline vs. passive IPS deployment

---

## Traditional vs. NGFW Rule Comparison

| Traffic Type | Traditional Firewall (port-based) | NGFW (application-aware) |
|-------------|----------------------------------|--------------------------|
| Web (HTTP) | TCP port 80 | Application: HTTP |
| Secure web (HTTPS) | TCP port 443 | Application: HTTPS |
| SSH | TCP port 22 | Application: SSH |
| Remote Desktop | TCP port 3389 | Application: Microsoft RDP |
| DNS | UDP port 53 | Application: DNS |
| NTP | UDP port 123 | Application: NTP |

NGFW rules can reference either port numbers **or** application names — or both. A rule that allows "Microsoft SQL Server traffic regardless of port" is only possible with an NGFW.

---

## Firewall Rule Base Structure

### How Rules Are Evaluated

```
[Traffic arrives at firewall]
          |
          ▼
[Compare against Rule 1] → match? → apply action (allow/deny)
          |
          ↓ no match
[Compare against Rule 2] → match? → apply action
          |
          ↓ no match
[Continue down rule list...]
          |
          ↓ no match after all rules
[Implicit deny — traffic dropped]
```

- Rules are evaluated **top to bottom**
- The **first matching rule** applies — evaluation stops
- More **specific rules go at the top**; broader rules go lower
- **Implicit deny** at the bottom catches everything not explicitly allowed

### Rule Base Parameters

A complete firewall rule may specify:

| Parameter | Description |
|-----------|-------------|
| **Rule number / name** | Identifier for the rule |
| **Source IP** | Where the traffic originates |
| **Destination IP** | Where the traffic is going |
| **Remote port** | Source port of the traffic |
| **Local port** | Destination port on the internal system |
| **Protocol** | TCP, UDP, ICMP |
| **User** | Specific user or group (NGFW) |
| **Application** | Specific application name (NGFW) |
| **URL / web category** | Specific URL or content category (NGFW) |
| **Action** | Allow or Deny |

---

## Sample Firewall Rule Base

| Rule | Remote IP | Remote Port | Local Port | Protocol | Action |
|------|-----------|-------------|------------|----------|--------|
| 1 | Any | Any | 22 | TCP | Allow (SSH) |
| 2 | Any | Any | 80 | TCP | Allow (HTTP) |
| 3 | Any | Any | 443 | TCP | Allow (HTTPS) |
| 4 | Any | Any | 3389 | TCP | Allow (RDP) |
| 5 | Any | 53 | Any | UDP | Allow (DNS) |
| 6 | Any | 123 | Any | UDP | Allow (NTP) |
| 7 | Any | N/A | N/A | ICMP | Deny (ping) |
| — | — | — | — | — | **Implicit deny (all other)** |

**Reading this rule base:**
- Rules 1–6 allow specific inbound services
- Rule 7 explicitly denies ICMP (ping) from the outside
- Anything not matching rules 1–7 is dropped by the implicit deny

---

## Access Control Lists (ACLs)

A **firewall rule base** is an implementation of an **Access Control List (ACL)** — a structured list of permit/deny rules applied to traffic. ACLs can exist in:

- Firewalls (primary use)
- Routers (restrict routing between subnets)
- Operating systems (file and directory permissions)
- Switches (port-level access control with 802.1X)

The firewall ACL shown above is a classic example — each rule is an ACL entry with source, destination, port, protocol, and action.

---

## Screened Subnet (DMZ)

A **screened subnet** (also called a DMZ) is a network segment that holds publicly accessible services — web servers, mail servers, DNS — separated from the internal network.

```
[Internet]
    |
[External Firewall] ← controls what enters from internet
    |
[Screened Subnet (DMZ)]
  Web servers, mail servers, DNS
    |
[Internal Firewall] ← prevents DMZ traffic from reaching internal network
    |
[Internal Network]
  Confidential data, internal systems
```

**Why this matters:**
- Internet traffic can reach the screened subnet — this is intentional
- Internet traffic **cannot** reach the internal network — firewalls block this
- Even if a DMZ server is compromised, the attacker faces the internal firewall before reaching sensitive data

---

## IPS Signatures

An **IPS (Intrusion Prevention System)** monitors traffic in real time and compares it against a database of attack signatures. When traffic matches a signature, the IPS can block it.

### How IPS Signatures Work

A signature defines:
- Specific traffic patterns (byte sequences, headers, payload content)
- Protocol behavior anomalies
- Known attack indicators (e.g., Conficker worm traffic patterns)

When traffic matches the signature, the IPS blocks the connection before it reaches the destination.

### Signature-Based vs. Anomaly-Based Detection

| Method | How It Works | Catches |
|--------|-------------|---------|
| **Signature-based** | Matches traffic against known attack patterns | Known attacks with specific indicators |
| **Anomaly-based** | Detects deviations from expected behavior | Novel attacks matching known categories (e.g., any SQL injection, not just known ones) |

Most modern IPS systems use both methods together.

### Managing Large Rule Sets

Enterprise IPS systems may contain **thousands of signatures**. Managing individual rules at this scale requires grouping:

| Management Approach | Description |
|--------------------|-------------|
| **Rule groups** | Bundle related signatures (e.g., all database injection signatures) into a named group |
| **Group-level policy** | Set a single allow/block decision for the entire group |
| **Auto-categorization** | IPS software automatically assigns signatures to groups |
| **False positive tuning** | Customize individual rules within groups to reduce false positives |

**Example group policy:** "Block all traffic matching any signature in the 'SQL Injection' group."

### IPS False Positives

Because IPS rule sets are large and traffic patterns vary, false positives are common. Management involves:

1. Identifying which signatures are triggering false positives
2. Adjusting those specific rules (reducing sensitivity or creating exceptions)
3. Maintaining the balance between security and operational impact

---

## Firewall as Multi-Service Platform

Modern firewalls — particularly NGFWs — serve multiple security functions simultaneously:

| Function | Description |
|----------|-------------|
| **Traffic filtering** | Allow/deny based on port or application |
| **VPN endpoint / concentrator** | Terminate site-to-site or remote access VPN tunnels |
| **Layer 3 routing** | Route traffic between network segments |
| **NAT** | Translate between internal and external IP addresses |
| **Integrated IPS** | Apply IPS signatures to traffic passing through |
| **URL filtering** | Block access to specified website categories |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Rule evaluation** | Top to bottom; first match applies; implicit deny at bottom catches everything else |
| **Specific before general** | More specific rules at the top of the rule base |
| **Implicit deny** | Anything not explicitly allowed is automatically dropped |
| **NGFW vs. traditional** | NGFW uses application name; traditional uses port number |
| **Screened subnet / DMZ** | Separates public-facing servers from internal network; both have firewall protection |
| **IPS signatures** | Match known attack patterns; can be signature-based or anomaly-based |
| **IPS rule groups** | Bundle thousands of signatures for manageable policy decisions |
| **False positive tuning** | Ongoing process of adjusting IPS rules to reduce noise without missing real threats |
