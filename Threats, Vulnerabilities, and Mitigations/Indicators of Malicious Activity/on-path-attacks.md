# On-Path Attacks (Man-in-the-Middle)

## Overview

An **on-path attack** (also called a **man-in-the-middle attack**) places the attacker between two communicating devices. All traffic between the victims flows through the attacker, who can:

- **Observe** — read all data passing between the two parties
- **Modify** — alter data in transit before forwarding it
- **Block** — prevent communication entirely

What makes on-path attacks particularly dangerous is that **neither victim is aware the attack is taking place**. From each device's perspective, it is communicating normally with the other.

---

## ARP Poisoning

**ARP (Address Resolution Protocol)** resolves IP addresses to MAC (hardware) addresses on a local network. When a device wants to communicate with another device, it needs its MAC address — ARP provides this mapping.

ARP has **no authentication or security** — any device on the subnet can send ARP replies, and receiving devices will accept and cache them without verification.

### Normal ARP Process

```
[Laptop — IP: 192.168.1.9 / MAC: ...38:d5]
    |
    | "Who is 192.168.1.1? Send me your MAC address."
    | (broadcast to entire subnet)
    ▼
[Router — IP: 192.168.1.1 / MAC: ...BB:FE]
    |
    | "I am 192.168.1.1, my MAC is ...BB:FE"
    ▼
[Laptop caches: 192.168.1.1 → ...BB:FE]
[Laptop now communicates directly with the router]
```

### ARP Poisoning Attack

The attacker sends an **unsolicited ARP reply** — even though no request was made — telling the laptop that the router's MAC address is actually the attacker's MAC address:

```
[Attacker — IP: 192.168.1.14 / MAC: ...EE:FF]
    |
    | Sends fake ARP reply:
    | "I am 192.168.1.1, my MAC is ...EE:FF"
    ▼
[Laptop updates cache: 192.168.1.1 → ...EE:FF]  ← poisoned
```

The same process is performed against the router — telling it that the laptop's MAC address is the attacker's.

**Result:**

```
[Laptop] ──► sends traffic to "router" ──► [Attacker] ──► forwards to [Router]
[Router] ──► sends traffic to "laptop" ──► [Attacker] ──► forwards to [Laptop]
```

All traffic flows through the attacker. Both devices believe they are communicating with each other. The attacker sees everything — and can modify any data in transit.

### Why ARP Poisoning Works

- ARP caches are updated whenever an ARP reply is received — no request required
- There is no cryptographic verification of ARP replies
- The attack only requires the attacker to be on the same subnet as the victims

---

## On-Path Browser Attack (Man-in-the-Browser)

A more targeted variant moves the attack entirely onto the **victim's own device**. Malware or a Trojan installed on the victim's machine acts as a **proxy inside the browser**, intercepting traffic before encryption and after decryption.

### Why This Bypasses Encryption

Network-level on-path attacks can be defeated by encrypting traffic (HTTPS, TLS). An on-path browser attack sidesteps this entirely — because it captures data **on the device itself**, before the browser encrypts it for transmission.

```
[Victim types credentials into browser]
          |
          ▼ (captured here — before encryption)
[Malware proxy — running on victim's device]
          |
          ▼ (traffic encrypted normally and sent to bank)
[Bank server — receives legitimate, valid request]
```

From the bank's perspective, the login appears completely normal. From the victim's perspective, the site looks and behaves normally. The malware is invisible to both.

### What the Attacker Does Next

Once credentials are captured, the malware can silently open additional browser sessions in the background — sessions the victim never sees:

| Action | Example |
|--------|---------|
| **Account takeover** | Log into banking portal and initiate fund transfers |
| **Financial fraud** | Make purchases at online retailers using saved payment methods |
| **Credential harvesting** | Capture credentials for every site the victim logs into |
| **Session hijacking** | Reuse authenticated session tokens to access services without re-entering credentials |

---

## ARP Poisoning vs. On-Path Browser Attack

| Property | ARP Poisoning | On-Path Browser Attack |
|----------|--------------|----------------------|
| **Location** | Network-level — attacker on same subnet | Device-level — malware on victim's machine |
| **Defeats encryption?** | No — HTTPS traffic is encrypted end-to-end | Yes — captures data before encryption |
| **Victim awareness** | None | None |
| **Requires physical proximity?** | Yes — same network segment | No — malware can be installed remotely |
| **Traffic visibility** | All traffic between poisoned devices | All browser traffic on the infected device |

---

## Defenses

### Against ARP Poisoning

| Defense | Description |
|---------|-------------|
| **Dynamic ARP Inspection (DAI)** | Network switch feature that validates ARP packets against a trusted table — rejects spoofed replies |
| **Static ARP entries** | Manually configure ARP entries for critical devices; they cannot be overwritten by spoofed replies |
| **Network segmentation** | Limits the scope of ARP poisoning to a single segment |
| **Encrypted protocols (HTTPS, SSH)** | Even if traffic is intercepted, encryption prevents the attacker from reading content |
| **VPNs** | Encrypts all traffic end-to-end regardless of network-level interception |

### Against On-Path Browser Attacks

| Defense | Description |
|---------|-------------|
| **Anti-malware software** | Detects and removes browser-hijacking Trojans before they can capture credentials |
| **Keep OS and browser updated** | Patches the vulnerabilities malware exploits to install |
| **Browser integrity extensions** | Some security tools detect unexpected proxy behavior in the browser |
| **Multi-factor authentication** | Captured credentials alone are insufficient if MFA is required for login |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **On-path attack** | Attacker positioned between two victims — reads, modifies, or blocks all traffic invisibly |
| **ARP poisoning** | Sends fake ARP replies to redirect local network traffic through the attacker's device |
| **ARP has no security** | Any device on the subnet can send ARP replies; receiving devices accept them without verification |
| **On-path browser attack** | Malware on the victim's device intercepts browser traffic before encryption — network-level encryption provides no protection |
| **Key defense (network)** | Dynamic ARP Inspection; encrypted protocols |
| **Key defense (device)** | Anti-malware; MFA; kept-current software |
