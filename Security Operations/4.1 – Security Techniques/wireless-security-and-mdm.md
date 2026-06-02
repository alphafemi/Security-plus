# Wireless Security & Mobile Device Management

## Overview

Wireless networks and mobile devices introduce security challenges that don't exist with wired infrastructure — signals pass through walls, devices roam across locations, and anyone in physical proximity can potentially interact with the network. Securing wireless environments requires understanding the radio frequency landscape, managing devices at scale, and protecting data transmitted over the air.

---

## Wireless Site Surveys

A **site survey** provides a detailed understanding of the wireless environment — both your own access points and those outside your control.

### Why Conduct a Site Survey

| Reason | Detail |
|--------|--------|
| **Understand coverage** | Identify signal strength across all areas of the facility |
| **Find interference** | Detect other access points using overlapping channels |
| **Optimize channel selection** | Configure your APs to minimize conflict with neighboring networks |
| **Identify rogue APs** | Discover unauthorized access points on or near your network |
| **Track changes over time** | Repeat surveys on a schedule to catch new interference sources |

### Site Survey Tools

| Tool | What It Shows |
|------|--------------|
| **Heat map software** | Visual representation of signal strength across a floor plan — strong signal in red/yellow, weak in blue |
| **Wireless survey tools** | List of nearby SSIDs, BSSIDs, channels, frequency bands, and signal metrics |
| **OS built-in tools** | Basic wireless adapter information and visible networks |
| **Third-party tools (e.g., NetSpot)** | Comprehensive AP discovery, signal metrics, and channel analysis |
| **Spectrum analyzer** | Shows all RF signals on a frequency — including non-Wi-Fi sources like microwave ovens |

> A spectrum analyzer is valuable when interference is suspected but not visible in Wi-Fi tools — it reveals every signal on the frequency, regardless of source.

---

## Wireless Security Threats

| Threat | Description |
|--------|-------------|
| **Unencrypted networks** | Public Wi-Fi (coffee shops, hotels) transmits data in the clear — use VPN |
| **Traffic monitoring** | Attacker in range can capture unencrypted wireless traffic |
| **On-path attack** | Attacker intercepts and relays traffic between device and AP (ARP poisoning, rogue AP) |
| **Deauthentication attack** | Forged management frames disconnect devices from the network |
| **RF jamming** | Transmitting interference on wireless frequencies causes denial of service |
| **Rogue access point** | Unauthorized AP on the network provides attackers easy network access |

> See also: [Wireless Attacks](wireless-attacks.md) for deauthentication and RF jamming detail.

---

## Mobile Device Management (MDM)

**MDM** platforms allow administrators to centrally manage, monitor, and enforce security policies on all mobile devices in the organization — both company-owned and personally owned.

### MDM Capabilities

| Capability | Description |
|-----------|-------------|
| **Policy enforcement** | Push security configurations (screen lock, PIN requirements, encryption) to all devices |
| **Application management** | Require specific apps to be installed; block prohibited apps |
| **Feature control** | Disable device features (camera, USB) in specific locations or contexts |
| **Data segmentation** | Partition corporate data separately from personal data |
| **Remote wipe** | Delete corporate data (or the entire device) remotely if lost or stolen |
| **Compliance monitoring** | Detect devices that don't meet security requirements |

### Data Segmentation on Mobile Devices

MDM can create logical partitions on a device:

```
[Corporate Segment] ← managed by MDM; admin can wipe independently
  - Email, calendar, corporate apps
  - Encrypted and policy-enforced

[Personal Segment] ← user's private data; not visible to IT
  - Personal photos, contacts, apps
```

If an employee leaves the organization, IT can remotely wipe the corporate segment without affecting personal data.

---

## Mobile Device Ownership Models

| Model | Abbreviation | Description |
|-------|-------------|-------------|
| **Bring Your Own Device** | BYOD | Employee's personal device used for both work and personal purposes |
| **Corporate Owned, Personally Enabled** | COPE | Company purchases and assigns the device; both corporate and personal use permitted |
| **Choose Your Own Device** | CYOD | Company purchases the device; employee selects from an approved list of options |

### BYOD Considerations

- Devices must meet company requirements to be enrolled in MDM
- Employee privacy must be protected — IT should not have visibility into personal data
- Clear policies must address what happens when the employee leaves or upgrades their device

### COPE Considerations

- Company controls the device fully — MDM enrollment is mandatory
- Personal use is permitted but within company policy
- Device management follows the same model as corporate laptops and workstations

---

## Wireless Technology Security Concerns

### Cellular (4G / 5G)

| Risk | Description |
|------|-------------|
| **Location tracking** | Cell towers triangulate device location — can be used for tracking |
| **Traffic interception** | Sophisticated attackers can intercept cellular communications |
| **IMSI catchers** | Fake cell towers (stingrays) can intercept calls and texts |

The distributed cell structure means devices can be reached from anywhere — maintaining patches and encryption is critical.

### Wi-Fi

| Risk | Description |
|------|-------------|
| **Unencrypted networks** | Public Wi-Fi exposes all unencrypted traffic to anyone nearby |
| **Evil twin AP** | Attacker creates a rogue AP with the same SSID as a trusted network |
| **On-path attack** | Attacker in range can intercept traffic |

**Mitigation:** Always use VPN on untrusted networks; prefer networks with WPA3 encryption.

### Bluetooth (PAN — Personal Area Network)

Bluetooth enables short-range device connections (headsets, smartwatches, keyboards, peripherals) without cables.

| Risk | Description |
|------|-------------|
| **Unauthorized pairing** | A malicious device could attempt to pair and access data |
| **Bluejacking** | Sending unsolicited messages via Bluetooth |
| **Bluesnarfing** | Unauthorized access to data on a device via Bluetooth |

**Mitigations:**
- Always use the formal pairing process — verify the pairing code before accepting
- Do not accept connection requests from unknown devices
- Disable Bluetooth when not in use
- Never auto-connect to unfamiliar Bluetooth devices

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Site survey** | Essential before deploying and periodically during operation — identifies coverage gaps and interference |
| **Heat map** | Visual representation of signal strength — helps optimize AP placement |
| **Spectrum analyzer** | Reveals all RF sources, not just Wi-Fi — useful for diagnosing unexplained interference |
| **MDM** | Centrally enforces security policies, manages apps, segments data, and enables remote wipe |
| **Data segmentation** | Corporate and personal data in separate partitions — IT can wipe one without affecting the other |
| **BYOD** | Employee device; must meet company requirements; privacy must be respected |
| **COPE** | Company-owned device; personal use permitted; fully managed |
| **Public Wi-Fi risk** | Unencrypted — always use VPN on untrusted networks |
| **Bluetooth pairing** | Always verify pairing codes; never auto-connect to unknown devices |
