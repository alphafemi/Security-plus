# Secure vs. Insecure Protocols

## Overview

Many common network protocols transmit data in plaintext — anyone who can capture the traffic can read it directly. Replacing insecure protocols with their encrypted equivalents is one of the most fundamental and impactful security improvements any organization can make.

---

## Insecure Protocols: Sending Data in the Clear

The following protocols send all data unencrypted. On any network — wired or wireless — anyone with access to the traffic can read credentials, content, and session data.

| Insecure Protocol | Purpose | Plaintext Includes |
|------------------|---------|-------------------|
| **Telnet** | Remote terminal access | All commands, keystrokes, credentials |
| **FTP** | File transfer | Credentials and file contents |
| **HTTP** | Web browsing | All page content, credentials submitted in forms |
| **SMTP** (unencrypted) | Email sending | Email content and headers |
| **IMAP** (unencrypted) | Email retrieval | Credentials and email content |
| **POP3** (unencrypted) | Email retrieval | Credentials and email content |

---

## The "Wall of Sheep"

At **DEF CON** — a major security conference — organizers monitor all wireless traffic. Any attendee using an insecure protocol has their data displayed publicly on the **"Wall of Sheep"**, showing:

- Username
- Partial password
- IP address
- Protocol in use (IMAP, HTTP, POP3, etc.)

This public display illustrates in real time how much information is exposed by insecure protocols — and that this exposure happens even at a conference attended by security professionals.

---

## Secure Protocol Replacements

For every insecure protocol, an encrypted alternative exists:

| Insecure Protocol | Port | Secure Replacement | Port |
|------------------|------|--------------------|------|
| **Telnet** | 23 | **SSH** (Secure Shell) | 22 |
| **FTP** | 21 | **SFTP** (SSH File Transfer Protocol) | 22 |
| **HTTP** | 80 | **HTTPS** | 443 |
| **IMAP** | 143 | **IMAPS** | 993 |
| **POP3** | 110 | **POP3S** | 995 |
| **SMTP** | 25 | **SMTPS** / SMTP with TLS | 465 / 587 |

> **If a secure alternative exists and the application supports it, there is no valid reason to use the insecure version.**

---

## Using Port Numbers as Encryption Indicators

Port numbers provide a quick indication of whether traffic is likely encrypted — but they are not a guarantee.

| Port | Protocol | Encrypted? |
|------|---------|-----------|
| 80 | HTTP | No |
| 443 | HTTPS | Yes |
| 22 | SSH / SFTP | Yes |
| 23 | Telnet | No |
| 21 | FTP | No |
| 993 | IMAPS | Yes |

### Important Caveat

A server listening on port 443 *should* be serving HTTPS — but a misconfigured server could still serve unencrypted content on that port. **The only way to confirm encryption is to capture and inspect the packets** — if the content is readable, it's not encrypted.

---

## Verifying Encryption with Packet Capture

A packet capture tool (such as Wireshark) reveals whether traffic is actually encrypted:

| What You See in the Capture | Conclusion |
|----------------------------|-----------|
| HTTP headers and content are readable | Traffic is **not encrypted** |
| TLS handshake; payload is unreadable ciphertext | Traffic **is encrypted** |
| Port 80 with readable content | HTTP — plaintext |
| Port 443 with readable content | Misconfigured — expected HTTPS but not encrypted |

Performing a packet capture on your own network periodically reveals which applications are sending data in the clear.

---

## Network-Level Encryption

When application-level encryption isn't available — or when all traffic needs to be protected regardless of application — network-level encryption provides a blanket solution.

### Wireless Encryption (WPA3)

Configuring WPA3 on a wireless access point encrypts all traffic transmitted over that wireless network — regardless of the application or protocol in use.

```
[No encryption (open AP)]
  All traffic visible to anyone within wireless range

[WPA3 encryption]
  All traffic encrypted on the wireless medium
  → Even insecure protocols (HTTP, Telnet) are protected over the air
```

**Limitation:** WPA3 only encrypts traffic on the wireless segment. Once traffic reaches the wired network or the internet, it is unencrypted again (if the application itself doesn't encrypt it).

### VPN (Virtual Private Network)

A VPN creates an encrypted tunnel between a device and a VPN concentrator — all traffic through the tunnel is encrypted regardless of application.

```
[User device]
  HTTP, Telnet, FTP, etc.
      |
      ▼ (all encrypted inside VPN tunnel)
[Internet — encrypted tunnel]
      |
      ▼
[VPN Concentrator — decrypts and forwards]
      |
      ▼
[Destination network — traffic continues as normal]
```

**Trade-offs:**
- Requires VPN client software on the device
- Requires a VPN concentrator (on-premises or third-party service)
- Only encrypts traffic to the VPN endpoint — traffic beyond that is unprotected if the application doesn't encrypt it

> **See also:** [VPN Technologies](vpn-technologies.md) for IPsec, SSL/TLS VPN, SD-WAN, and SASE

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Insecure protocols** | Telnet, FTP, HTTP, SMTP, IMAP, POP3 all send data in plaintext |
| **Wall of Sheep** | Real-world illustration of what insecure protocols expose to anyone capturing traffic |
| **Secure replacements** | SSH, SFTP, HTTPS, IMAPS, POP3S, SMTPS — use these instead |
| **Port numbers** | Provide a quick indicator of likely encryption, but a packet capture is the only confirmation |
| **WPA3** | Encrypts all wireless traffic on a network segment — but not beyond |
| **VPN** | Encrypts all traffic through a tunnel — requires client software and a concentrator |
| **Best practice** | Use application-level encryption (HTTPS, SSH, etc.) first; add network-level encryption (VPN) where needed |
