# Mobile Device Security

## Overview

Mobile devices present a unique and challenging security problem. Unlike desktop systems that sit in a fixed location on a managed network, mobile devices are:

- **Small and concealable** — easy to lose, steal, or smuggle past physical security
- **Constantly moving** — difficult to locate or manage at any given moment
- **Always connected** — continuously exposed to the internet, making remote access possible from anywhere in the world
- **Dual-purpose** — carry both personal and organizational data simultaneously

Despite having significant security built in at the hardware and OS level, mobile devices are frequently targeted through the circumvention of those protections.

---

## Jailbreaking & Rooting

Modern mobile operating systems are locked down by design — users do not have access to the underlying OS, and security controls are enforced at the firmware level. However, vulnerabilities in these systems can be exploited to replace or modify the operating system entirely.

| Platform | Term | What It Does |
|----------|------|-------------|
| **Apple iOS** | Jailbreaking | Replaces or modifies the iOS firmware to remove Apple's security restrictions |
| **Android** | Rooting | Gains root-level access to the Android OS, bypassing manufacturer and carrier restrictions |

### Why Users Do It

- Enable features not available in the stock OS
- Install applications not approved by the platform's app store
- Remove pre-installed apps or carrier restrictions
- Customize the OS beyond what the manufacturer allows

### Why It's a Security Problem

When a device is jailbroken or rooted, it effectively **bypasses all security controls** the organization has put in place through its **Mobile Device Manager (MDM)**. The MDM can no longer enforce:

- Application restrictions
- Encryption requirements
- Remote wipe capabilities
- Configuration policies

A jailbroken or rooted device is, from a security standpoint, an unmanaged device with full access to organizational data.

---

## Malicious Applications

Even without jailbreaking, a user who installs an untrusted application introduces significant risk. A single malicious app can:

- Access all data stored on the device
- Capture credentials or session tokens
- Exfiltrate sensitive organizational data to an attacker
- Operate silently in the background without the user's knowledge

### App Store Controls

To reduce this risk, organizations and platform vendors restrict where applications can be installed from:

| Source | Trust Level |
|--------|------------|
| **Official platform store** (Apple App Store, Google Play) | Reviewed and verified by the platform vendor |
| **Organization's internal app store or MDM library** | Approved and managed by the organization's IT department |
| **Third-party or unknown sources** | Unverified — significant risk of malicious content |

Organizations typically configure MDMs to enforce installation only from approved sources.

---

## Sideloading

**Sideloading** is the installation of applications outside of an official or approved app store — bypassing the vetting process entirely. On a stock device, this may be restricted or disabled by default. On a jailbroken or rooted device, sideloading becomes unrestricted.

```
[Stock Device]
      |
      ├── App Store only → controlled, reviewed apps
      └── Sideloading blocked by default

[Jailbroken / Rooted Device]
      |
      ├── App Store available
      └── Sideloading unrestricted → any app, from any source, with no review
```

Sideloaded applications carry all the same risks as any unverified software — they may contain malware, spyware, or data-exfiltration code that looks like a legitimate app on the surface.

---

## Policy & Enforcement

### Acceptable Use Policies (AUPs)

Organizations typically address mobile device security through formal policy documents — employee handbooks or **Acceptable Use Policies (AUPs)**. These policies commonly prohibit:

- Installing unauthorized operating systems (jailbreaking / rooting)
- Sideloading unapproved applications
- Connecting unmanaged personal devices to corporate systems without authorization

Violations of these policies can result in disciplinary action, up to and including termination, because a single compromised device represents a risk to the entire organization's data.

### Mobile Device Management (MDM)

An MDM platform gives organizations the ability to enforce security policies on enrolled devices:

| MDM Capability | Security Benefit |
|----------------|-----------------|
| **App allowlisting / blocklisting** | Restricts which apps can be installed |
| **Remote wipe** | Erases device data if lost or stolen |
| **Encryption enforcement** | Requires data-at-rest encryption |
| **Jailbreak / root detection** | Flags or quarantines compromised devices |
| **Configuration profiles** | Enforces VPN, Wi-Fi, and passcode policies |

> MDM controls are only effective on devices that have not been jailbroken or rooted. A modified OS can spoof compliance status or disable MDM enforcement entirely.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Mobile security challenges** | Small, mobile, always-connected devices carrying sensitive data are inherently difficult to secure |
| **Jailbreaking (iOS)** | Replaces iOS firmware to remove security restrictions — bypasses all MDM controls |
| **Rooting (Android)** | Gains root OS access on Android — same effect as jailbreaking |
| **Malicious apps** | A single bad application can expose all data on the device |
| **Sideloading** | Installing apps outside approved stores — removes the vetting layer that catches malicious software |
| **AUP** | Policy document that prohibits unauthorized OS modifications and app installations |
| **MDM** | Platform for enforcing device security policies — rendered ineffective by jailbreaking or rooting |
