# Resilience & Continuity of Operations

## Overview

**Resilience** is the ability of systems and services to continue operating — or recover quickly — when components fail or disasters occur. Building resilient infrastructure requires planning for failure at every level: individual components, servers, sites, platforms, and even cloud providers.

---

## High Availability (HA)

**High Availability** keeps services running continuously by maintaining redundant systems that are always active — so that if one fails, another immediately takes over without manual intervention.

### Redundancy vs. High Availability

| Approach | Description | Recovery Time |
|----------|-------------|--------------|
| **Redundancy** | Spare component stored — must be retrieved, installed, and configured when needed | Minutes to hours |
| **High Availability** | Redundant component is already running — takes over automatically | Seconds or less |

HA costs more because additional hardware must be purchased, powered, and maintained — but the reduction in downtime often justifies the investment for critical services.

---

## Server Clustering

**Server clustering** connects multiple servers so they function as a single logical unit. Users see one server; the cluster manages distribution and failover internally.

```
[Users]
   |
[Switch]
   |
[Server Cluster: Server A | Server B | Server C]
              ↕           ↕           ↕
         [Shared Storage — single source of truth for all cluster nodes]
```

### Characteristics

- All servers in the cluster run **identical operating systems** for interoperability
- Servers write to **shared storage** rather than local drives — ensures all nodes have current data
- Nodes can be **added or removed in real time** to scale capacity
- If a node fails, the remaining nodes absorb its load automatically

---

## Load Balancing

**Load balancing** distributes traffic across multiple servers using a central load balancer. Unlike clustering, individual servers are unaware of each other — only the load balancer knows about all of them.

```
[Users]
   |
[Load Balancer] ← single point of traffic management
   ├──► [Server A]
   ├──► [Server B]
   └──► [Server C]
```

### Characteristics

- Servers can run **different operating systems** — the load balancer abstracts this
- Servers are **added or removed** from the pool as capacity needs change
- Load balancer **monitors server health** — automatically removes failed servers and redistributes traffic
- Convergence is typically transparent to users

> **Clustering vs. load balancing:** Clustered servers know about each other; load-balanced servers do not. Clustering shares state across nodes; load balancing routes requests independently.

---

## Site Resiliency

When a physical disaster affects an entire location, **site resiliency** provides a geographically separate facility that can assume operations.

### Recovery Site Types

| Type | Description | Cost | Readiness |
|------|-------------|------|-----------|
| **Hot site** | Exact replica of the primary data center — hardware, applications, and data fully synchronized | Highest | Immediate failover |
| **Warm site** | Some equipment and partial data available — additional hardware and data must be brought in | Medium | Hours to days |
| **Cold site** | Empty building with power and lighting only — all equipment and data must be transported | Lowest | Days to weeks |

### Choosing a Recovery Site

| Consideration | Detail |
|--------------|--------|
| **Disaster frequency** | Rare disasters may not justify the cost of a hot site |
| **Recovery time objective** | How quickly must services be restored? |
| **Geographical dispersion** | Recovery site must be far enough away to avoid the same disaster (e.g., different state for hurricane protection) |
| **Staffing** | Remote sites need employees — transportation plans must account for disaster conditions |
| **Return process** | Once the disaster passes, moving back to the primary site requires its own plan |

### Site Resiliency Flow

```
[Normal operations at primary site]
          |
          | (disaster declared)
          ▼
[Failover to recovery site]
  - Hot: immediate, seamless
  - Warm: hours of preparation
  - Cold: days of setup
          |
          | (disaster resolved)
          ▼
[Failback to primary site]
```

---

## Platform Diversity

**Platform diversity** uses different operating systems or technology platforms for different functions, reducing the risk that a single vulnerability affects all systems simultaneously.

| Deployment | Platform Mix | Benefit |
|-----------|-------------|---------|
| **Data center** | Linux + Windows servers | A Windows-specific vulnerability doesn't affect Linux servers |
| **Clients** | Windows + macOS | OS-specific malware affects fewer endpoints |
| **Mixed environment** | Multiple OS families | Lateral spread of OS-specific exploits is limited |

A vulnerability discovered in Windows is almost certainly not present in Linux or macOS — and vice versa. Using multiple platforms limits the blast radius of any single OS-level flaw.

---

## Multi-Cloud Resilience

Using multiple cloud providers provides resilience against provider-specific outages and security incidents.

| Scenario | Single Cloud | Multi-Cloud |
|----------|-------------|------------|
| **Provider outage** | All services down | Services on other provider continue |
| **Provider security incident** | Full exposure | Limits data and service exposure |
| **Geographic outage** | Region-specific failure possible | Services distributed across providers and regions |

**Considerations:**
- Application services and data can be distributed across providers
- Requires managing different tools, APIs, and configurations per provider
- A security issue with one provider does not necessarily affect others

---

## Continuity of Operations Planning (COOP)

**COOP** plans for scenarios where technology is completely unavailable — ensuring that critical business functions can continue through **non-technical fallback processes**.

### Why COOP Is Necessary

Modern operations depend heavily on technology. When IT systems fail completely, organizations without COOP plans grind to a halt. COOP identifies manual alternatives for every critical function.

### COOP Fallback Examples

| Normal Process | COOP Fallback |
|---------------|--------------|
| Automated transaction processing | Manual transaction forms with physical sign-off |
| Electronic receipts from POS | Paper receipts |
| Automated credit card approval | Phone authorization with card issuer |
| Digital communication | Phone, fax, or courier |

> COOP plans must be **designed and tested before they are needed**. A disaster is not the time to figure out how to process transactions without a computer.

---

## Resiliency Strategy Comparison

| Strategy | Protects Against | Implementation Complexity |
|----------|-----------------|--------------------------|
| **High Availability** | Component and server failure | Medium — requires duplicate active systems |
| **Server clustering** | Server failure; supports scaling | Medium-High — shared storage and OS consistency required |
| **Load balancing** | Server failure; traffic spikes | Medium — load balancer configuration |
| **Hot site** | Site-level disaster | High — duplicate infrastructure, constant sync |
| **Warm site** | Site-level disaster | Medium — partial infrastructure ready |
| **Cold site** | Site-level disaster (tolerate longer outage) | Low — building only |
| **Platform diversity** | OS-specific vulnerabilities | Low — purchasing and configuration decisions |
| **Multi-cloud** | Provider outage or security incident | High — multiple vendor relationships |
| **COOP** | Complete technology unavailability | Medium — process design and training |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **High availability** | Redundant systems always active — automatic failover with minimal downtime |
| **Server clustering** | Multiple servers appear as one; shared storage keeps all nodes synchronized |
| **Load balancing** | Central load balancer distributes traffic; servers are unaware of each other |
| **Hot site** | Exact duplicate of primary data center — immediate failover at highest cost |
| **Warm site** | Partial infrastructure ready — hours of preparation needed |
| **Cold site** | Empty facility — lowest cost, longest recovery time |
| **Platform diversity** | Multiple OS families limit the reach of OS-specific vulnerabilities |
| **Multi-cloud** | Multiple providers prevent single-provider outage or incident from taking everything down |
| **COOP** | Non-technical fallback processes for when all technology fails — must be planned in advance |
