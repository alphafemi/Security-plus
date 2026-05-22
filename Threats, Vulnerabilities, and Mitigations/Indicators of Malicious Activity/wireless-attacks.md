# Wireless Attacks: Deauthentication & RF Jamming

## Overview

Wireless networks introduce attack surfaces that wired networks do not have — anyone within radio range can interact with the signal. Two common wireless denial-of-service attacks exploit this: **deauthentication attacks** that kick specific devices off a network, and **RF jamming** that disrupts wireless communication for everyone in range.

---

## Wireless Deauthentication Attacks

### How Wireless Management Frames Work

Wi-Fi devices communicate with access points not just to send data, but to manage the connection itself. This happens through **management frames** — control messages sent in the background to:

- Discover and connect to access points
- Authenticate and associate with a network
- Disconnect when done

These frames operate invisibly to the user, but are essential to maintaining the wireless connection.

### The Vulnerability

In earlier versions of the **802.11 specification**, management frames were sent **completely unencrypted** — in the clear, with no authentication or integrity protection. This means anyone within range can:

- Read all management frame details (receiver/transmitter addresses, SSID, supported rates, etc.)
- **Forge** management frames, including deauthentication frames

A **deauthentication frame** tells a device it has been disconnected from the network. If an attacker continuously sends forged deauthentication frames targeting a specific device, that device is repeatedly kicked off the network and cannot reconnect — a targeted wireless denial of service.

### Attack Process

```
[Attacker]
    |
    1. Capture wireless traffic to identify target device MAC address
    |     (e.g., using airodump-ng)
    |
    2. Identify access point BSSID and target device MAC
    |
    3. Send continuous forged deauthentication frames
    |     (e.g., using aireplay-ng -0)
    |
    ▼
[Target Device]
    |
    - Receives deauth frame → drops connection
    - Attempts to reconnect → receives another deauth frame
    - Cycle repeats indefinitely while attack continues
    ▼
[Target device cannot use the network]
```

No special access is required — only proximity to the wireless network and knowledge of the target device's MAC address, which is visible in standard wireless traffic captures.

### The Fix: 802.11ac and Management Frame Protection

The IEEE 802.11 committee addressed this vulnerability in the **802.11ac standard and newer** by encrypting sensitive management frames by default:

| Frame Type | Pre-802.11ac | 802.11ac+ |
|-----------|-------------|-----------|
| **Disassociate** | Unencrypted | Encrypted |
| **Deauthenticate** | Unencrypted | Encrypted |
| **Channel switch announcements** | Unencrypted | Encrypted |
| **Beacons / Probes** | Unencrypted | Still unencrypted (required before encryption can be established) |
| **Initial authentication / association** | Unencrypted | Still unencrypted (required to set up the encrypted session) |

Some management frames must remain in the clear because they occur before the encrypted session can be established — but the frames most critical to session stability (deauth, disassociate) are now protected.

---

## RF Jamming

### What It Is

**RF (Radio Frequency) jamming** is a broader denial of service that does not target a specific device — it disrupts wireless communication for **all devices** in range of the jamming signal. Rather than exploiting a protocol vulnerability, it works by overwhelming the wireless medium with noise.

When the **signal-to-noise ratio** drops low enough, devices can no longer reliably decode transmissions from the access point — effectively severing all wireless communication in the affected area.

### Common Jamming Techniques

| Method | Description |
|--------|-------------|
| **Constant jamming** | Continuous transmission of interfering signal |
| **Random data flooding** | Transmits random data continuously across the wireless frequency |
| **Legitimate frame flooding** | Sends large volumes of valid-looking Wi-Fi frames, saturating the medium |
| **Reactive jamming** | Silent when the network is idle; activates only when traffic is detected — harder to troubleshoot |

### Unintentional Jamming

Not all jamming is malicious. Common everyday sources of 2.4 GHz interference include:

- **Microwave ovens** — emit significant 2.4 GHz radiation during operation
- **Fluorescent lights** — can generate interference across wireless frequencies
- **Baby monitors, cordless phones** — older devices operating on the same frequency band

If interference is intermittent and correlated with appliance use, it is likely unintentional. Sustained or reactive jamming with no obvious environmental cause suggests a deliberate attack.

### Locating a Jammer: The Fox Hunt

Because jamming requires physical proximity, the attacker must be **somewhere nearby**. Locating the source is a process borrowed from amateur radio, called a **fox hunt**:

```
[Detect jamming signal]
          |
          ▼
[Use directional antenna to identify signal direction]
          |
          ▼
[Move toward the signal — strength increases]
          |
          ▼
[Add attenuator to reduce signal strength and improve directional accuracy]
          |
          ▼
[Triangulate signal origin from multiple positions]
          |
          ▼
[Locate and disable the jamming source]
```

A directional antenna identifies the direction of the signal; an attenuator helps when close-range signal strength makes direction difficult to discern.

---

## Comparison

| Property | Deauthentication Attack | RF Jamming |
|----------|------------------------|------------|
| **Target** | Specific device(s) | All devices in range |
| **Method** | Forged management frames | Overwhelming RF interference |
| **Protocol exploit** | Yes — unencrypted 802.11 management frames | No — physical layer disruption |
| **Required proximity** | Yes | Yes |
| **Fixed by 802.11ac?** | Yes (for protected frame types) | No — physical mitigation required |
| **Detection** | Packet capture showing forged frames | RF spectrum analysis; signal hunting |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Management frames** | Background control messages that manage Wi-Fi connections; unencrypted in older 802.11 specifications |
| **Deauthentication attack** | Forged deauth frames continuously kick a target device off the wireless network |
| **802.11ac fix** | Sensitive management frames (deauth, disassociate) are now encrypted by default in 802.11ac and newer |
| **RF jamming** | Disrupts wireless for all nearby devices by lowering the signal-to-noise ratio |
| **Reactive jamming** | Activates only when traffic is detected — difficult to troubleshoot |
| **Fox hunt** | Directional antenna + attenuator technique used to physically locate a jamming source |
