# Network Security Architecture

## Overview

How a network is physically and logically structured has a direct impact on its security. **Air gaps** prevent any connectivity between isolated segments. **VLANs** provide logical isolation on shared hardware. **Software Defined Networking (SDN)** abstracts network functions into software — enabling dynamic, cloud-native infrastructure. Each of these approaches addresses the fundamental question: how do we prevent an attacker who gains access to one part of the network from reaching another?

---

## Air Gaps

An **air gap** is a physical isolation between two network segments — no cable, no wireless link, no shared switch. If an attacker gains access to one side of an air gap, they have no path to the other side.

### Use Cases

| Scenario | Why Air Gap |
|----------|------------|
| **Web servers vs. database servers** | Prevents a compromised web server from directly reaching the database layer |
| **Managed service providers** | Physically isolates one customer's systems from another |
| **High-security environments** | Critical systems that must never be reachable from general networks |

### Physical Air Gap Example

```
[Customer A Switch]          [Customer B Switch]
  ├── Device A1                ├── Device B1
  ├── Device A2                ├── Device B2
  └── Device A3                └── Device B3

No connection between switches = no path between customers
```

Devices within each switch can communicate with each other. No traffic can flow between the two switches because there is no physical connection.

### Limitation

Physical air gaps require a separate physical switch per isolated segment. At scale — 100 customers requiring isolation — this means 100 switches. This does not scale efficiently, which is where VLANs solve the problem.

---

## VLANs (Virtual Local Area Networks)

A **VLAN** provides logical network segmentation on a single physical switch. Ports assigned to different VLANs cannot communicate directly — the switch enforces this separation in software.

### VLAN vs. Physical Isolation

| Property | Physical Air Gap | VLAN |
|----------|-----------------|------|
| **Hardware required** | Separate switch per segment | One switch can host many VLANs |
| **Isolation level** | Physical | Logical |
| **Scalability** | Poor — one switch per segment | Excellent — many VLANs per switch |
| **Security** | Absolute physical isolation | Strong logical isolation (requires proper config) |
| **Cost** | Higher | Lower |

### How VLANs Work

```
[Single Physical Switch]
  Port 1 → VLAN 10 (Customer A)
  Port 2 → VLAN 10 (Customer A)
  Port 3 → VLAN 20 (Customer B)
  Port 4 → VLAN 20 (Customer B)

VLAN 10 devices can communicate with each other.
VLAN 20 devices can communicate with each other.
VLAN 10 and VLAN 20 cannot communicate without going through a router or firewall.
```

VLANs achieve the same logical isolation as separate physical switches — at a fraction of the cost and complexity.

---

## Software Defined Networking (SDN)

**SDN** separates the functions of a physical network device into distinct software layers — enabling network infrastructure to be defined, deployed, and modified as code rather than hardware configuration.

### The Three Planes of Operation

Every network device (switch, router, firewall) performs three distinct types of work:

| Plane | Function | Examples |
|-------|----------|---------|
| **Data Plane** (Infrastructure Layer) | Forwards traffic from one device to another | Packet forwarding, NAT, encryption, trunking |
| **Control Plane** | Manages how the data plane makes forwarding decisions | Routing tables, dynamic routing protocols, ARP tables, address translation tables |
| **Management Plane** | Accepts configuration changes from administrators | SSH CLI access, SNMP, API calls |

### How the Planes Interact

```
[Administrator]
      |
      | (SSH / SNMP / API)
      ▼
[Management Plane] ── configuration changes ──►
                                                 [Control Plane]
                                                  (routing tables,
                                                   lookup tables)
                                                      |
                                                      | (forwarding decisions)
                                                      ▼
[Network Traffic] ──────────────────────────► [Data Plane]
                                               (packet forwarding,
                                                NAT, encryption)
```

### Why SDN Matters for Cloud Security

By converting these three planes into software, SDN allows:

- **Dynamic provisioning** — create firewalls, switches, and routers in seconds via API or GUI
- **Scalability** — spin up additional infrastructure instantly as load demands
- **Cloud-native deployment** — network devices exist as software instances, not physical hardware
- **Consistent policy enforcement** — security rules applied programmatically, reducing human configuration error

### SDN in a Cloud Architecture

A typical cloud application deployment using SDN:

```
[Internet]
    |
    ▼
[Firewall 1] ← dynamically provisioned via SDN
    |
    ▼
[Load Balancer]
    |
    ├──► [Web Server A]
    ├──► [Web Server B]
    |
    ▼
[Firewall 2] ← dynamically provisioned via SDN
    |
    ▼
[Database Server]
```

Each firewall is a software-defined instance that can be created, configured, and scaled through a cloud management console — no physical hardware required. Adding a new security layer takes minutes rather than weeks.

---

## Combining These Technologies

Real-world network security architectures typically layer all three approaches:

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Physical separation** | Air gap | Absolute isolation for the most sensitive segments |
| **Logical separation** | VLANs | Cost-effective isolation across shared infrastructure |
| **Dynamic infrastructure** | SDN | Cloud-native, programmable network security at scale |

A managed service provider, for example, might use:
- **VLANs** to isolate standard customers on shared switches
- **Air gaps** for customers with contractual requirements for physical isolation
- **SDN firewalls** in their cloud infrastructure to enforce per-customer security policies

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Air gap** | Physical isolation — no connectivity between segments; prevents lateral movement but does not scale |
| **VLAN** | Logical isolation on shared hardware — scalable, cost-effective alternative to physical air gaps |
| **SDN** | Network functions defined as software — enables dynamic, cloud-native infrastructure provisioning |
| **Data plane** | Forwards traffic; handles NAT, encryption, and trunking |
| **Control plane** | Manages routing tables and forwarding decisions |
| **Management plane** | Accepts administrative configuration via SSH, SNMP, or API |
| **Cloud SDN benefit** | Firewalls and other network devices can be provisioned programmatically in seconds |
