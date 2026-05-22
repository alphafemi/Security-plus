# Physical Attacks

## Overview

Digital security controls — firewalls, encryption, authentication systems — protect against network-based attacks. But an attacker with **physical access to a system can bypass all of them**. Someone who can physically touch a computer can circumvent the operating system, extract storage media, reset credentials, or simply walk out with the hardware.

Physical security is not a separate concern from digital security — it is a foundational layer beneath it.

> "Door locks only keep out honest people." Physical deterrents must be supported by layered controls, monitoring, and response capabilities.

---

## Brute Force Physical Access

**Physical brute force** refers to forcing entry through physical barriers — doors, windows, walls, or enclosures — to reach systems or infrastructure. Unlike its digital counterpart (password guessing), physical brute force requires no technical knowledge.

### Attack Scenarios

- Forcing open a server room door or data center entrance
- Breaking a window to access a wiring closet or equipment room
- Removing a wall panel or ceiling tile to access a restricted area

### Evaluation Exercise

Security teams should periodically ask: *How difficult would it be for an attacker to physically force their way into our data center?* The answer often reveals weak points that purely digital security assessments miss.

### Mitigations

- Reinforced doors and frames with commercial-grade locks
- Security cameras covering all entry points
- Motion sensors and alarms on perimeter access points
- Security guards or access-controlled man-traps at entry
- Regular physical security assessments

---

## RFID Cloning

**RFID (Radio Frequency Identification)** is the technology behind most modern access badges and key fobs used for physical entry control. If an attacker can clone a legitimate RFID credential, they gain the same physical access as the card's rightful owner.

### Why It's Easy

| Factor | Detail |
|--------|--------|
| **Equipment cost** | RFID cloners are available for under $50 USD on consumer retail sites |
| **Speed** | Reading and copying a card takes only seconds |
| **Proximity** | An attacker with a concealed reader can clone a card without the owner noticing — on public transit, in an elevator, or in a crowd |

### The Proximity Attack

A documented technique involves an attacker carrying a concealed RFID reader in a crowded location (e.g., a commuter train). By standing near a target, the reader captures the credential from the badge in the target's pocket or bag — no interaction required.

### Mitigation: Multi-Factor Authentication

A cloned RFID card alone is not sufficient if **multi-factor authentication (MFA)** is required for entry. Even with a duplicate card, the attacker would still need:

- A **PIN** the cardholder knows
- A **biometric** factor (fingerprint, iris scan) the cardholder possesses

Physical MFA for access control renders cloned credentials far less useful.

### Additional Mitigations

- Use RFID-blocking sleeves or wallets to prevent unauthorized reads
- Implement MFA at all sensitive access points
- Monitor for duplicate credential use — the same badge presenting at two distant locations simultaneously is a strong indicator of cloning
- Regularly audit access logs for anomalous entry patterns

---

## Environmental Attacks

When direct physical access to systems is blocked, attackers may target the **environment that the systems depend on**. Data centers and server rooms rely on stable power, cooling, and suppression systems — all of which can be disrupted from outside the secure perimeter.

### Power Disruption

An attacker may be able to cut or disrupt power to a data center **from outside the building** — at external electrical panels, utility access points, or power distribution equipment.

**Effect:** Systems go offline immediately. Even with UPS (uninterruptible power supply) and generator backup, a sustained power attack can exhaust backup capacity.

### HVAC / Cooling Disruption

HVAC systems controlling data center cooling frequently receive **lower security priority** than the IT systems they support — making them an attractive target.

**Attack method:** Gain access to HVAC controls and disable cooling.

**Effect:**
```
[Cooling disabled]
      |
      v
[Server temperatures rise]
      |
      v
[Systems reach thermal threshold and auto-shutdown]
      |
      v
[Data center effectively offline — no damage required]
```

This achieves a denial of service without touching a single server, and the attacker may not even need to enter the building.

### Fire Suppression System

Fire suppression systems in data centers are designed to protect equipment — but they can also be weaponized:

- **Unauthorized activation** of suppression systems can disrupt operations
- Some suppression agents (e.g., halon or inert gas systems) displace oxygen, requiring evacuation and creating downtime
- Access to suppression controls may be less secured than access to the servers themselves

### Environmental Attack Summary

| Target | Attack Method | Effect |
|--------|--------------|--------|
| **Power** | Disable external electrical supply | Immediate system shutdown |
| **Cooling (HVAC)** | Disable cooling systems | Thermal shutdown of servers |
| **Fire suppression** | Trigger suppression system | Forced evacuation, potential equipment disruption |

### Mitigations

- Physically secure all utility control panels — treat them with the same rigor as server room access
- Implement monitoring and alerting on HVAC, power, and suppression systems
- Require MFA and physical access controls for environmental system controls
- Ensure UPS, generator, and redundant power systems are tested regularly
- Position security cameras on all external utility access points

---

## Summary

| Attack Type | Key Takeaway |
|------------|-------------|
| **Physical brute force** | Forced entry bypasses all digital controls — physical barriers must be evaluated as part of security assessments |
| **RFID cloning** | Access badges can be duplicated for under $50 in seconds — MFA is the most effective countermeasure |
| **Power disruption** | Cutting external power can take a data center offline without entering the building |
| **HVAC attack** | Disabling cooling causes thermal shutdowns — HVAC controls are often under-secured |
| **Fire suppression attack** | Triggering suppression systems can force evacuation and cause service disruption |
| **Physical MFA** | Even with a cloned credential, a PIN or biometric requirement stops unauthorized entry |
