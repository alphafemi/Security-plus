# Recovery Testing & Security Simulations

## Overview

Having a disaster recovery plan is necessary but insufficient — the plan must also be **tested regularly**. Organizations that only discover gaps in their recovery process during an actual disaster are at far greater risk than those that practice recovery in controlled conditions. Testing also reveals whether security controls like phishing defenses actually work in practice.

---

## Recovery Testing Principles

Effective recovery testing follows consistent principles:

| Principle | Description |
|-----------|-------------|
| **Regular schedule** | Test often enough that staff understand the process and it becomes familiar |
| **Defined scope** | Clearly limit what is included in the test to avoid impacting production systems |
| **Specific scenario** | Each test should represent a realistic failure or disaster scenario |
| **Defined time frame** | Tests have a start and end time — avoid open-ended exercises |
| **Post-test evaluation** | After each test, review what worked, what didn't, and update the plan accordingly |

---

## Tabletop Exercise

A **tabletop exercise** simulates a disaster recovery scenario as a group discussion — walking through each step of the recovery plan without actually executing it.

### How It Works

Participants gather (around a table or virtually) and verbally work through what they would do in the scenario. Each department's role is discussed in sequence — IT, operations, communications, legal, and any other affected team.

### Benefits

- **Low cost** — no infrastructure needs to be moved or configured
- **Identifies gaps** — missing steps, unclear responsibilities, or incorrect assumptions surface through discussion
- **Cross-functional alignment** — ensures all departments' plans are compatible with each other
- **Fast iteration** — a tabletop can be conducted in hours; findings can be incorporated into the plan before the next exercise

### Limitation

A tabletop does not test whether systems actually work — only whether the plan makes logical sense.

---

## Failover Test

A **failover test** verifies that redundant systems actually take over when a primary system fails — and that the transition is seamless to users.

### Ideal Failover Characteristics

- **Automatic** — no manual intervention required
- **Transparent** — users are redirected to backup systems without noticing a change
- **Fast** — convergence happens quickly enough that no service interruption is experienced

### What to Test

| Component | Failover Method |
|-----------|----------------|
| **Internet connections** | Multiple ISP connections; BGP or routing failover |
| **Routers** | Redundant routers with failover protocol (HSRP, VRRP) |
| **Firewalls** | Active/passive HA pair — secondary takes over if primary fails |
| **Switches** | Redundant switches with spanning tree or link aggregation |
| **Servers** | Load-balanced pool — failed server removed, traffic redistributed |

### Redundant Failover Architecture

```
[ISP A] ──► [Router A] ──►
                           [Redundant Firewalls]
[ISP B] ──► [Router B] ──►        |
                           [Redundant Switches]
                                   |
                          [Load Balancer] ──► [Server Farm]
```

If any single component fails — ISP connection, router, firewall, or switch — an alternative path exists. Load balancers handle server-level failures within the server farm.

---

## Security Simulations

Beyond infrastructure recovery, organizations should test whether their **security controls actually work** when faced with real attack conditions.

### Phishing Simulation

Phishing training tells users what to look for — but phishing simulations test whether they actually apply that training.

**Process:**
1. Create a realistic phishing email — something relevant and enticing to employees
2. Send it to all users (or a selected group)
3. Monitor click rates, credential submissions, and report rates
4. Identify users who clicked and assign additional targeted training
5. Verify whether security controls (email filtering, URL blocking) caught the message before it reached users

**What it measures:**
- Effectiveness of email security controls
- Percentage of users susceptible to clicking
- Effectiveness of user security awareness training

> A phishing simulation that reaches zero users because email filtering caught it is a success — even if no one clicked.

### Other Simulation Types

| Simulation | What It Tests |
|-----------|--------------|
| **Password reset process** | Whether the reset workflow can be exploited for social engineering |
| **Data exfiltration attempt** | Whether DLP and monitoring tools detect data leaving the network |
| **Malware execution test** | Whether EDR and antivirus catch known-malicious behavior |
| **Unauthorized access attempt** | Whether alerts fire when someone attempts to access restricted resources |

---

## Parallel Processing

**Parallel processing** uses multiple CPUs or processing nodes simultaneously, distributing workload across them. It provides both performance and resilience.

### Forms of Parallel Processing

| Configuration | Description |
|--------------|-------------|
| **Multi-core processor** | Single device with multiple CPU cores handling work simultaneously |
| **Multi-system cluster** | Multiple computers working together on a shared workload |
| **Cloud-based scaling** | Multiple cloud instances processing transactions in parallel |

### Resilience Benefit

```
[Normal: 4 processors active]
  CPU A | CPU B | CPU C | CPU D → handles full load

[CPU B fails]
  CPU A | CPU C | CPU D → absorbs CPU B's share
                         (each works harder, but service continues)
```

If one processor becomes unavailable, the remaining processors absorb its share of the workload — maintaining availability, potentially with reduced throughput.

---

## Test Type Comparison

| Test Type | Cost | Realism | Identifies |
|-----------|------|---------|-----------|
| **Tabletop exercise** | Low | Low (discussion only) | Plan gaps, coordination issues |
| **Failover test** | Medium | High (real systems) | Technical failover reliability |
| **Phishing simulation** | Low | High (real attack conditions) | User susceptibility, email control effectiveness |
| **Data exfiltration simulation** | Medium | Medium-High | DLP and monitoring gaps |
| **Full disaster recovery drill** | High | Very high | Everything — end-to-end recovery capability |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Recovery testing** | Must be done regularly — discovering gaps during an actual disaster is too late |
| **Tabletop exercise** | Low-cost group walkthrough of the recovery plan — identifies logical gaps without touching production |
| **Failover test** | Verifies that redundant systems actually take over automatically and transparently |
| **Phishing simulation** | Real-condition test of user awareness and email security controls |
| **Parallel processing** | Distributes workload across multiple CPUs or systems — improves performance and provides resilience if one processor fails |
| **Post-test evaluation** | Every test should result in plan improvements — recovery planning is a continuous cycle |
