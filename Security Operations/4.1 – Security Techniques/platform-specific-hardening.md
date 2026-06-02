# Platform-Specific Hardening

## Overview

Every platform — mobile devices, workstations, servers, network devices, cloud systems, and specialized hardware — requires hardening tailored to its specific characteristics. Default configurations are rarely secure; hardening closes the gaps. This page provides a platform-by-platform hardening reference.

> **See also:** [System Hardening](system-hardening.md) for OS-level hardening techniques · [IoT Security, Firmware & Legacy Systems](iot-firmware-and-legacy-systems.md) for EOL/EOSL considerations

---

## Mobile Devices

| Hardening Control | Description |
|------------------|-------------|
| **Apply patches** | Install manufacturer security updates promptly — closes exploitable vulnerabilities |
| **Data segmentation** | Separate corporate data from personal data on the device — limits breach scope |
| **MDM enforcement** | Use Mobile Device Management to push security policies, monitor compliance, and remotely wipe if needed |
| **Use manufacturer hardening guides** | Most mobile OS vendors publish security guidance specific to their platform |

Data segmentation is particularly valuable in BYOD (Bring Your Own Device) environments — if an attacker compromises the personal segment, corporate data remains isolated.

---

## Workstations (Windows, macOS, Linux)

| Hardening Control | Description |
|------------------|-------------|
| **Patch OS and applications** | Monthly patch cycles (e.g., Microsoft Patch Tuesday) — apply promptly |
| **Remove unused software** | Every installed application is a potential vulnerability; remove anything not in use |
| **Use hardening guides** | OS vendors and CIS Benchmarks provide platform-specific guidance |
| **Enable EDR** | Endpoint Detection and Response provides behavioral monitoring beyond signature-based AV |
| **Enforce least privilege** | Users should not run with administrative rights by default |

---

## Network Infrastructure (Switches, Routers, Firewalls)

Network devices run purpose-built embedded operating systems — not Windows or Linux — which means:

- Limited direct OS access
- Patches come exclusively from the manufacturer
- Default credentials are a common, documented vulnerability

| Hardening Control | Description |
|------------------|-------------|
| **Change default credentials** | First and most critical step — default credentials are publicly known |
| **Implement authentication** | Local authentication or centralized (RADIUS, TACACS+) |
| **Apply manufacturer patches** | Network device patches are infrequent — when one is released, treat it as high priority |
| **Restrict management access** | Limit which IP addresses can access the management interface |
| **Disable unused services and ports** | Every enabled service is an attack surface |

---

## Cloud Infrastructure

Cloud management workstations and cloud-hosted systems require hardening like any other system — with additional cloud-specific considerations.

| Hardening Control | Description |
|------------------|-------------|
| **Least privilege for cloud services** | Applications, services, and users should have minimum required permissions |
| **Audit network settings and access policies** | Review what is publicly accessible and what is not |
| **Install EDR on cloud instances** | Cloud VMs are vulnerable to the same malware as physical systems |
| **Back up to a separate provider** | A cloud provider outage should not take down your backups |
| **Harden the management workstation** | The cloud management console has full access — the machine running it must be secured |

---

## Servers (Windows, Linux)

| Hardening Control | Description |
|------------------|-------------|
| **Apply OS patches and service packs** | Keep current with security patches from the OS vendor |
| **Password policy enforcement** | Minimum length, complexity, and expiration requirements |
| **Least privilege for all accounts** | Disable unused accounts; limit service accounts to what they need |
| **Restrict network access** | Firewall rules or host-based policies limiting which systems can connect |
| **Install EDR / antivirus** | Behavioral and signature-based protection on the server itself |

---

## SCADA / Industrial Control Systems (ICS)

SCADA systems manage large-scale industrial equipment — power generation, manufacturing, water treatment. The security model is built around strict isolation.

| Hardening Control | Description |
|------------------|-------------|
| **Air-gap from corporate network** | No direct connectivity between SCADA network and business/internet networks |
| **No internet access** | SCADA systems should never be directly internet-reachable |
| **Strict physical access controls** | Centralized control rooms with limited personnel access |
| **Real-time monitoring** | Distributed control systems monitor equipment state continuously |
| **Dedicated security assessments** | SCADA environments require specialized expertise — not standard IT security tools |

> SCADA compromise can cause physical damage to equipment or infrastructure. Security failures here have consequences far beyond data loss.

---

## Embedded Systems

Embedded systems run purpose-built hardware and firmware for a single function — smartwatches, TVs, industrial sensors, medical equipment.

| Hardening Control | Description |
|------------------|-------------|
| **Apply patches immediately when available** | Embedded system patches are rare; when released, they address serious issues |
| **Network segmentation** | Place embedded devices on a dedicated network segment isolated from other systems |
| **Firewall protection** | Add a firewall in front of the embedded device segment |
| **Limit network connectivity** | Embedded devices should only communicate with the systems they need to |

Because access to the underlying OS is minimal or impossible, network-level controls are often the primary defense for embedded systems.

---

## Real-Time Operating Systems (RTOS)

RTOS platforms require deterministic timing — used in automobiles, military equipment, and industrial machinery where delays have physical consequences.

| Hardening Control | Description |
|------------------|-------------|
| **Isolate from other networks** | No other system should be able to affect RTOS timing or performance |
| **Minimum services** | Run only the services explicitly required — nothing else |
| **Firewall or host-based security** | Filter any network communication the RTOS must perform |
| **No general-purpose software** | RTOS environments should not run antivirus or other general tools that affect timing |

---

## IoT Devices

IoT devices (thermostats, cameras, smart locks, lighting controllers) are designed by manufacturers focused on functionality, not security.

| Hardening Control | Description |
|------------------|-------------|
| **Patch immediately** | IoT patches address security issues in devices with minimal built-in protection |
| **Change default credentials** | Factory defaults are publicly documented and widely known |
| **Segment onto a dedicated network** | If an IoT device is compromised, the attacker should be contained to the IoT segment |
| **Monitor traffic** | Unexpected outbound connections from IoT devices may indicate compromise |
| **Disable unused features** | Remote management interfaces not in use should be disabled |

---

## Hardening Priority Summary

| Platform | Highest Priority Controls |
|----------|--------------------------|
| **Mobile** | Patches, data segmentation, MDM |
| **Workstations** | Patches, remove unused software, EDR, least privilege |
| **Network devices** | Change defaults, patch, restrict management access |
| **Cloud** | Least privilege, audit access, EDR on instances, backup to separate provider |
| **Servers** | Patches, least privilege, restrict network access, EDR |
| **SCADA/ICS** | Air-gap, no internet, physical access controls |
| **Embedded systems** | Patch when available, segment, firewall |
| **RTOS** | Isolate, minimum services, no general-purpose tools |
| **IoT** | Patch, change defaults, segment, monitor |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Default configurations** | Rarely secure — every platform requires hardening before deployment |
| **Manufacturer hardening guides** | Use them — vendors provide platform-specific guidance based on known risks |
| **Network segmentation** | Isolating specialized devices (IoT, SCADA, embedded) limits breach scope |
| **Patch priority** | Rare patches for embedded systems and network devices carry high significance |
| **SCADA isolation** | Air-gap is the gold standard — compromise has physical, not just data, consequences |
| **Cloud hardening** | Cloud systems need the same controls as physical ones — plus cloud-specific least privilege |
