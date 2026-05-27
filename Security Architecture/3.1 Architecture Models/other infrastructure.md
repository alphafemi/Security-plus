# Availability, Resilience & Infrastructure Planning

## Overview

**Availability** is the measure of whether systems and resources are accessible to authorized users when needed. It is one of the three pillars of the CIA triad (Confidentiality, Integrity, Availability) and a foundational goal of any IT infrastructure. Planning for availability means planning not just for normal operations, but for failure — and for recovery.

---

## Availability Metrics

### Uptime as a Percentage

Availability is commonly expressed as a percentage of uptime over a period:

| Availability | Downtime per Year |
|-------------|-------------------|
| 99% ("two nines") | ~3.65 days |
| 99.9% ("three nines") | ~8.76 hours |
| 99.99% ("four nines") | ~52.6 minutes |
| 99.999% ("five nines") | ~5.26 minutes |

The difference between 99.9% and 99.999% is roughly 8.5 hours of downtime per year — a significant operational and financial distinction.

### Mean Time to Repair (MTTR)

**MTTR** measures how long it takes to restore a system after failure. It is a key measure of operational resilience.

**MTTR is affected by:**
- Time to identify the root cause (hardware, software, or configuration failure)
- Time to source replacement components
- Time to restore from backup or reinstall
- Time to validate the restored system

**Example — malware recovery:**

| Recovery Method | Time | Cost |
|----------------|------|------|
| Reinstall OS from original media | ~60 minutes | Labor intensive |
| Restore from pre-built system image | ~10 minutes | Faster, requires prior image creation |

Both produce the same outcome — a clean, working system — but the image-based recovery is significantly faster and less expensive. **Recovery planning should be done before an incident, not during one.**

---

## Responsiveness & Elasticity

### Responsiveness

**Responsiveness** describes how quickly a system returns a result after a request. For interactive applications, human perception of delay begins at roughly 100–200ms.

Responsiveness is complex to measure because:
- A single transaction may involve multiple backend steps
- Different functions within the same application may respond at different speeds
- Load affects response time non-linearly — a small increase in traffic can cause a large drop in responsiveness

### Elasticity

**Elasticity** is the ability to automatically scale resources up or down to match demand — without manual intervention.

```
[Normal load]    → base application instance
[Load increases] → additional compute instances spin up automatically
[Load decreases] → extra instances are removed to reduce cost
```

**Security implication:** Security monitoring tools must scale with the application. If additional instances spin up but security coverage doesn't extend to them, those instances are unmonitored.

---

## Cost of Infrastructure

Infrastructure cost is rarely a single number. A complete cost model includes:

| Cost Category | Examples |
|--------------|---------|
| **Initial purchase / setup** | Hardware, licenses, cloud provisioning fees |
| **Ongoing maintenance** | Vendor support contracts, internal staff time |
| **Replacement / repair** | Hardware failure, emergency remediation |
| **Capital expense (CapEx)** | Depreciation of physical assets |
| **Operational expense (OpEx)** | Recurring cloud or service costs |
| **Tax implications** | Depreciation schedules, capitalization rules |

When evaluating infrastructure decisions, all of these categories must be considered — not just the upfront purchase price.

---

## Application Components & Orchestration

Modern applications are rarely a single system. A typical application instance may include:

- Web server (handles client requests)
- Application server (business logic)
- Database server (data storage)
- Caching server (performance)
- Firewall (security boundary)
- Load balancer (traffic distribution)

### Orchestration

**Orchestration** automates the deployment of these components as a complete, coordinated stack — typically in a cloud environment.

```
[Orchestration system receives deployment request]
          |
          ▼
[Automatically provisions: web servers, app servers,
 database, cache, firewall, load balancer]
          |
          ▼
[Application instance running — no manual configuration required]
```

This is one of the core advantages of cloud infrastructure: entire application stacks can be deployed, scaled, or torn down programmatically.

**Project management consideration:** Any missed dependency (wrong region, insufficient budget, missing access permission) can delay the entire deployment. Planning must account for all components before deployment begins.

---

## Cybersecurity Insurance

Organizations can transfer some financial risk from security events to a third party through **cybersecurity insurance**.

### What It May Cover

| Event | Coverage |
|-------|---------|
| **Ransomware** | Financial losses from encrypted systems or paid ransoms |
| **Business downtime** | Revenue lost during an outage caused by a security event |
| **Data breach** | Notification costs, regulatory fines, credit monitoring for affected individuals |
| **Legal proceedings** | Attorney fees and settlement costs from customer lawsuits |

> Cybersecurity insurance does not prevent attacks — it provides financial recovery after one. It is a complement to, not a replacement for, security controls.

---

## Patch Management as Infrastructure Planning

Patch management is a recurring operational process, not a one-time task. It must be built into the infrastructure plan from the beginning.

### Standard Patching Cycle

```
[New patch released by vendor]
          |
          ▼
[Test in non-production environment]
          |
          ▼
[Verify no breaking changes]
          |
          ▼
[Deploy to production]
          |
          ▼
[Monitor for issues post-deployment]
```

### Unpatched and Unpatchable Systems

Some systems cannot be easily patched:

| System Type | Example | Risk |
|-------------|---------|------|
| **Embedded systems** | HVAC controllers, time clocks | Purpose-built, no update mechanism |
| **Legacy systems** | EOSL operating systems | No more patches from vendor |
| **Air-gapped systems** | Industrial controllers | No network connectivity for updates |

**Mitigation for unpatchable systems:**
- Place a firewall between the device and the rest of the network
- Restrict network access to only necessary connections
- Monitor traffic to and from the device for anomalies

---

## Power Infrastructure

Power is the most fundamental dependency in any infrastructure — without it, nothing runs.

### Power Planning Considerations

- Engage a licensed electrician to assess current usage and future capacity needs
- Data centers have fundamentally different power requirements than office buildings
- In high-density areas, multiple power providers may be available — consider redundant feeds

### Backup Power Options

| Option | Use Case |
|--------|---------|
| **UPS (Uninterruptible Power Supply)** | Immediate bridge — provides power during brief outages and graceful shutdown time |
| **Generator** | Sustained backup — powers some or all systems during extended outages |
| **Dual power feeds** | Redundant utility connections from different substations |

---

## Compute Resources

In cloud-based architectures, compute is broken into scalable units — **Compute Engines** or equivalent services — rather than fixed physical hardware.

| Property | On-Premises | Cloud |
|----------|------------|-------|
| **Scaling** | Requires purchasing and installing new hardware | Add or remove compute instances in minutes |
| **Cost model** | CapEx — fixed hardware cost | OpEx — pay for what you use |
| **Flexibility** | Fixed capacity after purchase | Elastic — scale to any size |

The compute layer is where the actual processing of application data occurs. Security controls, monitoring, and patch management must extend to every compute instance — including those added dynamically during scale-out events.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Availability** | Expressed as uptime percentage; five nines = ~5 minutes downtime/year |
| **MTTR** | Mean time to repair — plan recovery procedures before incidents, not during |
| **Responsiveness** | Speed of request/response; affected by application complexity and load |
| **Elasticity** | Auto-scaling to match demand — security tools must scale with the application |
| **Orchestration** | Automated deployment of full application stacks; requires complete upfront planning |
| **Cybersecurity insurance** | Transfers financial risk — supplements security controls, does not replace them |
| **Patch management** | Recurring process; unpatchable systems need compensating controls |
| **Power infrastructure** | Most fundamental dependency; plan UPS, generators, and redundant feeds |
| **Compute** | The processing layer; elastic in the cloud; security coverage must follow scaling |
