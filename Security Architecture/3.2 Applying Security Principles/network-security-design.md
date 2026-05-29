# Network Security Design

## Overview

Secure network design is not a single technology or product — it is an architectural discipline that determines how devices are placed, how zones are defined, and how traffic is controlled. Every network is unique, but the principles that make networks secure are consistent: segment thoughtfully, minimize the attack surface, and assume attackers will find every gap.

---

## Security Zones

A **security zone** is a logical grouping of network resources based on their access level and trust relationship — not just their IP address range. Zones make security rules easier to understand, apply, and maintain across large and complex networks.

### Common Zone Names

| Zone Name | Typical Contents | Trust Level |
|-----------|-----------------|-------------|
| **Trusted / Inside** | Internal workstations, servers, staff devices | High |
| **Untrusted / Internet / Outside** | The public internet; anything external | None |
| **Screened / DMZ** | Public-facing servers (web, mail, DNS) | Limited |
| **Servers** | Internal application or database servers | Medium-High |
| **Management** | Administrative interfaces and jump servers | Very High |

### Example Zone Rules

Security rules are expressed in terms of zones rather than individual IP addresses — making them more readable and maintainable:

| Rule | Meaning |
|------|---------|
| Allow Trusted → Untrusted | Internal users can browse the internet |
| Allow Untrusted → Screened | External users can reach public-facing servers |
| Deny Untrusted → Trusted | No direct internet access to internal systems |
| Allow Trusted → Servers | Internal users can reach application servers |
| Deny Screened → Trusted | Public-facing servers cannot initiate connections to internal network |

### Zone Design Evolution

**Simple two-zone model:**
```
[Internet] ── [Firewall] ── [Internal Network]
  (Untrusted)                  (Trusted)
```

**Three-zone model with screened subnet (DMZ):**
```
[Internet] ── [Firewall] ── [Screened Zone (DMZ)] ── [Firewall] ── [Inside Zone]
 (Untrusted)                 Web, Mail, DNS servers                   Internal systems
```

The three-zone model allows external users to reach public-facing servers while ensuring that even a compromised DMZ server cannot directly reach internal systems.

---

## The Attack Surface

The **attack surface** is the total set of points through which an attacker could attempt to gain unauthorized access. Reducing the attack surface is a continuous security objective.

### Common Attack Surface Components

| Entry Point | Examples |
|------------|---------|
| **Application code** | Vulnerabilities in web apps, APIs, or software |
| **Open ports** | Unnecessary services listening on network ports |
| **Authentication** | Weak passwords, no MFA, vulnerable login processes |
| **Human error** | Misconfigured firewall rules, accidental data exposure |
| **Network connectivity** | Unencrypted links, exposed network drops, accessible cabling |

### Minimizing the Attack Surface

| Action | Effect |
|--------|--------|
| **Audit and patch application code** | Eliminates known software vulnerabilities |
| **Close unnecessary ports** | Removes entry points that serve no legitimate purpose |
| **Review firewall rules regularly** | Catches accidental misconfigurations before attackers do |
| **Monitor traffic in real time** | Detects unexpected connections and unauthorized access |
| **Apply least privilege** | Limits what can be accessed if one component is compromised |

> A perfectly patched server with a single misconfigured firewall rule can still be breached. The attack surface includes every possible entry — not just the obvious ones.

---

## Secure Network Connectivity

### Physical Cabling Security

Network drops in offices, conference rooms, and hallways are potential physical access points. An attacker who can plug into a network drop has immediate access to that network segment.

**Physical protections:**
- Lock unused network ports at the switch level (disable the port)
- Secure cabling runs so taps cannot be placed undetected
- Use locked patch panels and wiring closets

**Logical protections:**
- Enable 802.1X port authentication — devices must authenticate before network access is granted
- Monitor for unexpected MAC addresses appearing on switch ports

### Application-Level Encryption

Even if an attacker gains physical access and taps the network cabling, **application-level encryption** ensures captured packets contain no readable data.

If all traffic between systems is encrypted (HTTPS, TLS, SSH), a physical wire tap yields only unreadable ciphertext.

### Remote Site & VPN Connectivity

Remote workers and branch offices connecting over the public internet require encrypted tunnels:

| Connection Type | Protocol | Use Case |
|----------------|---------|---------|
| **Site-to-site VPN** | IPsec | Permanent encrypted tunnel between two offices |
| **Remote access VPN** | SSL/TLS | Individual users connecting securely from outside |
| **VPN concentrator** | IPsec / SSL | Centralized device that terminates many remote VPN connections |

---

## Supporting Security Infrastructure

Beyond firewalls, several additional technologies contribute to a secure network design:

| Device / Technology | Security Role |
|--------------------|--------------|
| **Honeypot** | Decoy system designed to attract and detect attackers — reveals attack patterns and intent |
| **Jump server (bastion host)** | A hardened, monitored gateway that administrators must pass through to reach sensitive systems |
| **Network sensors** | Devices that monitor traffic for anomalies, intrusion attempts, and policy violations |
| **Load balancer** | Distributes traffic across servers; can also provide DDoS mitigation and SSL termination |
| **IDS/IPS** | Detects and/or blocks known attack patterns in network traffic |

---

## Putting It Together: Defense in Depth

A well-designed network does not rely on any single security control. Each layer compensates for the potential failure of the layers around it:

```
[Internet]
    |
[Perimeter Firewall] ← zone enforcement, traffic filtering
    |
[DMZ — Screened Zone]
  Web servers, mail, DNS
    |
[Internal Firewall] ← prevents DMZ → Inside traffic
    |
[Inside Zone]
  Application servers, databases
    |
  [802.1X on all switches] ← physical port security
  [IDS/IPS sensors] ← traffic monitoring
  [Jump server] ← controlled admin access
  [Application encryption] ← protects data even if network is tapped
```

Each layer must fail before the next one is tested. An attacker who bypasses the perimeter firewall still faces the internal firewall, the IDS/IPS, the 802.1X controls, and the application encryption.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Security zones** | Logical groupings by trust level — make rules more readable and maintainable than IP-based rules |
| **DMZ / Screened zone** | Buffer between the internet and internal systems — public-facing servers live here |
| **Attack surface** | Every possible entry point — code, ports, authentication, human error, and physical access |
| **Minimize attack surface** | Patch code, close ports, audit firewall rules, apply least privilege |
| **802.1X** | Port-level authentication — devices must authenticate before the network grants access |
| **Application encryption** | Protects data even if physical access to cabling is gained |
| **IPsec / VPN** | Encrypts site-to-site and remote access connections over the public internet |
| **Defense in depth** | Multiple overlapping security layers — each must fail independently for an attacker to succeed |
