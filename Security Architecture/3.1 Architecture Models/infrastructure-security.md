# Infrastructure Security

## Overview

Modern IT infrastructure spans cloud environments, virtualized data centers, containerized applications, IoT devices, industrial control systems, and embedded hardware. Each of these platforms has distinct security characteristics, attack surfaces, and management challenges. This page covers the key infrastructure types and their security implications.

---

## On-Premises vs. Cloud Security

Neither on-premises nor cloud is inherently more secure — each has trade-offs.

| Property | On-Premises | Cloud |
|----------|------------|-------|
| **Control** | Complete — all decisions made internally | Shared with provider per responsibility matrix |
| **Staff** | Internal IT team — higher cost, deep familiarity | Provider manages underlying infrastructure |
| **Hardware** | Must be purchased, configured, and maintained | No hardware to manage |
| **Customization** | Unlimited — change anything without third-party approval | Within provider's service boundaries |
| **Scaling** | Requires purchasing and installing new equipment | Instant, on-demand |
| **Data location** | Fully controlled | Governed by provider's data residency policies |

From an attacker's perspective, the location doesn't matter — the target is the data, regardless of where it lives.

---

## Centralized Security Management

As organizations grow across multiple locations, cloud providers, and OS platforms, managing security becomes increasingly complex. A **centralized management console** addresses this by providing a single view of the entire environment.

### Benefits

- Consolidated alerts from all systems in one place
- Unified log search across the entire organization
- Global patch and configuration management
- Single point for policy enforcement

### Trade-offs

| Risk | Description |
|------|-------------|
| **Single point of failure** | Losing the console loses visibility into all security events |
| **Scalability** | Larger organizations generate more logs and alerts — the console must scale accordingly |
| **Storage growth** | Log volume grows with the organization — storage must be planned for |

---

## Virtualization vs. Containerization

Both technologies allow multiple applications to run on shared hardware — but they differ significantly in how they achieve isolation.

### Virtualization

Each virtual machine runs its own complete **guest operating system** on top of a **hypervisor**:

```
[Physical Hardware]
       |
[Hypervisor] ← manages resource allocation between VMs
  ├── [Guest OS 1] → [App A]
  ├── [Guest OS 2] → [App B]
  └── [Guest OS 3] → [App C]
```

**Limitation:** If all three VMs run the same OS, you are still running three full, separate OS instances — significant overhead.

### Containerization

Containers share a **single host OS**. The container software (e.g., **Docker**) manages isolation between applications without requiring separate OS instances:

```
[Physical Hardware]
       |
[Host Operating System]
       |
[Container Software (Docker)]
  ├── [Container: App A]
  ├── [Container: App B]
  └── [Container: App C]
```

Each container includes everything the application needs to run **except** the OS — which is shared from the host.

### Comparison

| Property | Virtualization | Containerization |
|----------|--------------|-----------------|
| **OS per instance** | Yes — full guest OS per VM | No — all containers share host OS |
| **Isolation** | Strong — separate OS instances | Strong — isolated process spaces |
| **Overhead** | Higher — full OS per VM | Lower — no duplicate OS instances |
| **Startup time** | Slower — full OS boot | Fast — container launches in seconds |
| **Flexibility** | Run any OS on any hypervisor | Applications must target the host OS |
| **Best for** | Mixed OS environments | Uniform OS, microservices, cloud-native apps |

---

## IoT Security

**Internet of Things (IoT)** devices — thermostats, cameras, sensors, smart locks, wearables, industrial monitors — are designed for convenience, not security. The manufacturers excel at their primary function; security is often an afterthought.

### The Risk

A single compromised IoT device can provide an attacker with a foothold inside the network — the same foothold that was used in the Target breach (via HVAC systems).

### IoT Security Challenges

| Challenge | Description |
|-----------|-------------|
| **Weak default credentials** | Many IoT devices ship with default usernames and passwords |
| **Infrequent patches** | Manufacturers may take months or years to release security updates |
| **Limited security controls** | No antivirus, no EDR, no host-based firewall |
| **EOSL risk** | Manufacturers stop supporting older devices with no clear replacement path |
| **Network access** | IoT devices are network-connected by design — each one is a potential entry point |

### Mitigations

- Segment IoT devices onto a dedicated VLAN isolated from production systems
- Change default credentials immediately on all devices
- Monitor IoT device traffic for anomalies
- Inventory all IoT devices and track firmware versions

---

## SCADA / Industrial Control Systems (ICS)

**SCADA (Supervisory Control and Data Acquisition)** systems manage large-scale industrial equipment — manufacturing machinery, power generation, oil refineries, water treatment plants.

### Security Characteristics

| Property | Description |
|----------|-------------|
| **Critical impact** | Compromise of a power grid or refinery could affect an entire region |
| **Strict segmentation** | SCADA systems must be completely isolated from external networks |
| **High-value target** | Nation-state actors specifically target critical infrastructure |
| **Legacy systems** | Many SCADA installations run older, unpatched software |
| **Air-gapped where possible** | The most sensitive systems have no internet connectivity at all |

> Stuxnet — the nation-state worm developed to destroy Iranian nuclear centrifuges — specifically targeted SCADA systems. It is the most prominent example of ICS-focused malware.

---

## Real-Time Operating Systems (RTOS)

A **Real-Time Operating System** is used where timing is critical and delays could cause physical harm or failure — automotive braking systems, military equipment, manufacturing machinery.

Unlike standard operating systems (Windows, Linux), an RTOS is **deterministic** — a critical process can immediately take full priority over all other system resources.

### Security Approach

- RTOS environments are typically **self-contained and purpose-built** — external access is intentionally minimal
- No antivirus or general-purpose security tools are installed
- Security is achieved through isolation, controlled interfaces, and hardware design
- Any vulnerability in an RTOS carries serious physical consequences — reliability and security are inseparable

---

## Embedded Systems

An **embedded system** is a self-contained device where hardware and software are purpose-built for a single function. Examples include:

- Traffic light controllers
- Digital watches and fitness trackers
- Medical monitoring equipment
- Industrial sensors

### Security Characteristics

- Runs a single, fixed function — no user-installed applications
- No general-purpose OS interface accessible to users
- Limited attack surface by design — but firmware vulnerabilities still exist
- Updates may be difficult or impossible without physical access to the device

---

## High Availability (HA)

**High Availability** ensures that critical systems remain operational even when individual components fail — going beyond simple redundancy to provide continuous uptime.

### Redundancy vs. High Availability

| Property | Redundancy | High Availability |
|----------|-----------|------------------|
| **Definition** | Backup system exists | Backup system is always ready to take over instantly |
| **Failover time** | Manual — retrieve, install, configure | Automatic — seamless, often undetected by users |
| **Cost** | Lower | Higher |

### HA Configurations

| Configuration | Description |
|--------------|-------------|
| **Active/Passive** | One system handles all traffic; the other waits on standby; failover on primary failure |
| **Active/Active** | Both systems handle traffic simultaneously; either can absorb full load if the other fails |

### HA Cost Considerations

High availability compounds costs:

```
[2 firewalls for HA]
        ↓
[2 network infrastructures to support them]
        ↓
[2 power systems for full redundancy]
        ↓
[2 cooling systems, 2 uplinks, etc.]
```

Each layer of HA adds cost. Organizations must make a business decision: how much is downtime worth versus the cost of preventing it?

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **On-premises vs. cloud** | Both have trade-offs; attackers target data regardless of where it lives |
| **Centralized management** | Single console for visibility across the org — but creates a single point of failure |
| **Virtualization** | Full OS per VM — strong isolation, higher overhead |
| **Containerization** | Shared host OS — lower overhead, fast startup, cloud-native |
| **IoT security** | Convenient but often poorly secured — segment, change defaults, monitor |
| **SCADA/ICS** | Critical infrastructure — strictly segmented; compromise has physical consequences |
| **RTOS** | Deterministic OS for time-critical systems — self-contained, no general security tools |
| **Embedded systems** | Single-purpose hardware/software — limited attack surface but firmware vulnerabilities exist |
| **High availability** | Continuous uptime through redundant active systems — cost scales with each additional layer |
