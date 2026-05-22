# Denial of Service (DoS) & Distributed Denial of Service (DDoS)

## Overview

A **Denial of Service (DoS)** attack forces a service to become unavailable — either by overwhelming it with traffic, exploiting a vulnerability that causes it to fail, or simply removing a resource it depends on. The goal is not to steal data but to disrupt availability.

DoS can be deliberate or accidental, targeted or self-inflicted.

---

## Types of Denial of Service

### Intentional DoS

| Motivation | Description |
|-----------|-------------|
| **Service disruption** | Simply taking a target offline |
| **Competitive advantage** | Removing a competitor's services from the internet |
| **Smokescreen** | Distracting security teams while another attack is executed elsewhere |
| **Extortion** | Threatening sustained DoS unless payment is made |

### Unintentional (Self-Inflicted) DoS

DoS is not always caused by an attacker. Common self-inflicted scenarios include:

| Cause | Description |
|-------|-------------|
| **Network loop** | Two switches connected to each other in a loop without Spanning Tree Protocol creates a broadcast storm that takes down the network segment |
| **Bandwidth saturation** | Downloading a large file (e.g., a Linux ISO) on a limited connection consumes all bandwidth available to production applications |
| **Physical failure** | A water line above a data center ceiling bursting causes both physical damage and a complete service outage |
| **Power removal** | Simply unplugging a system is one of the most basic and effective DoS methods |

---

## Distributed Denial of Service (DDoS)

A single machine has limited bandwidth and processing power. A **DDoS** attack scales this by coordinating thousands or millions of devices simultaneously — all targeting a single victim.

### Botnets: The DDoS Engine

Attackers do not personally operate all of these devices. Instead, they plant malware on systems around the world, assembling them into a **botnet** — a network of compromised machines (bots) that execute commands from a central **command and control (C2)** server.

```
[Attacker — Command & Control]
          |
          | (single command: "attack target")
          |
    ┌─────┴──────┐
    ▼            ▼
[Bot 1]      [Bot 2]     ... [Bot N]
 (any        (any              (any
country)    country)          country)
    |            |               |
    └────────────┴───────────────┘
                 |
                 ▼
          [Target Server]
       (overwhelmed — offline)
```

**Example:** At its peak, the Zeus botnet controlled over **3.6 million compromised computers** — all deployable against any target with a single command.

### Why DDoS Is an Asymmetric Threat

The attacker expends minimal resources — a single command — while the victim must absorb traffic from millions of sources simultaneously. An attacker with limited infrastructure can take down organizations with far greater bandwidth and computing resources than their own.

---

## Amplification Attacks

Attackers have refined DDoS further by using **amplification** — sending small requests that generate large responses, multiplying the effective volume of attack traffic without proportionally increasing the attacker's own bandwidth usage.

### How Amplification Works

Several common internet protocols return significantly more data than was requested:

| Protocol | Amplification Factor (approx.) |
|----------|-------------------------------|
| **DNS** | Up to ~86× |
| **NTP** | Up to ~556× |
| **ICMP** | Varies |

### DNS Amplification Example

A DNS query for all records of a domain (`dig ANY isc.org`) sends approximately **15 characters** but receives back approximately **1,300 characters** in response — an amplification of roughly 86×.

An attacker who sends this query with a **spoofed source IP** (set to the victim's IP address) causes the DNS server to send the large response directly to the victim — not the attacker.

### DNS Amplification Attack Flow

```
[Attacker C2]
     |
     | (command: query these open DNS resolvers,
     |  spoof source IP = victim's IP)
     |
[Botnet — thousands of infected systems]
     |
     | (small DNS queries → open DNS resolvers)
     ▼
[Open DNS Resolvers — misconfigured, publicly accessible]
     |
     | (large DNS responses sent to spoofed IP = victim)
     ▼
[Victim Server — receives massive amplified traffic → offline]
```

**Key factors that enable this attack:**
- **Open DNS resolvers** — DNS servers that respond to queries from any source, rather than only trusted clients
- **IP spoofing** — the attacker forges the source address in DNS requests so responses flood the victim
- **Amplification ratio** — small queries produce large responses, multiplying attack volume

---

## Defenses Against DoS & DDoS

| Defense | Description |
|---------|-------------|
| **Keep systems patched** | Closes vulnerabilities that DoS attacks exploit to crash services |
| **Rate limiting** | Restricts the number of requests a single source can make in a given period |
| **Ingress filtering** | ISPs and network operators block spoofed source IP addresses, reducing amplification effectiveness |
| **Close open DNS resolvers** | Configure DNS servers to only respond to authorized clients |
| **DDoS mitigation services** | Cloud-based scrubbing centers absorb and filter attack traffic before it reaches the target |
| **Redundancy and failover** | Distributing services across multiple data centers reduces the impact of a single-point DoS |
| **Spanning Tree Protocol** | Prevents network loops that cause self-inflicted broadcast storm DoS |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **DoS** | Forces a service offline through overload, exploitation, or resource removal |
| **Self-inflicted DoS** | Network loops, bandwidth saturation, or physical failures can cause DoS without an attacker |
| **DDoS** | Coordinated attack from many systems simultaneously — far harder to block than single-source DoS |
| **Botnet** | A network of malware-infected machines controlled remotely; the infrastructure behind most DDoS attacks |
| **Zeus botnet** | Controlled 3.6 million compromised systems at peak — a single command could direct all of them |
| **Amplification** | Small requests that generate large responses are spoofed to flood a victim with traffic |
| **DNS amplification** | A 15-character query generates ~1,300 characters in response — ~86× amplification |
| **Open DNS resolvers** | Misconfigured DNS servers are the enabler of DNS amplification attacks — should be restricted |
