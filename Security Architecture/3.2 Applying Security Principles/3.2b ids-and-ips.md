# Intrusion Detection & Prevention Systems (IDS/IPS)

## Overview

**Intrusion Prevention Systems (IPS)** monitor network traffic in real time and actively block traffic identified as malicious. **Intrusion Detection Systems (IDS)** perform the same monitoring but can only alert — they cannot block. The distinction is critical: detection without prevention tells you an attack happened; prevention stops it.

---

## IDS vs. IPS

| Property | IDS | IPS |
|----------|-----|-----|
| **Monitors traffic** | Yes | Yes |
| **Alerts on threats** | Yes | Yes |
| **Blocks malicious traffic** | No | Yes |
| **Position in network** | Passive (out-of-band) | Active (inline) or Passive |
| **Network impact if it fails** | None — not in traffic path | Depends on fail mode |

### What They Detect

Both IDS and IPS can identify:

- **Known vulnerabilities** — exploits targeting specific OS or application CVEs
- **Generic attack patterns** — buffer overflows, SQL injection, cross-site scripting
- **Protocol anomalies** — traffic that violates expected protocol behavior
- **Behavioral indicators** — unusual traffic volumes, unexpected connection patterns

---

## Inline (Active) Monitoring

In an **inline** configuration, all traffic passes *through* the IPS. The device sits between two network segments — typically between a firewall and a core switch — and inspects every packet before forwarding it.

```
[Internet]
    |
[Firewall]
    |
[IPS — inline, active monitoring] ← all traffic passes through here
    |
[Core Switch]
    |
[Internal Network]
```

### Traffic Flow with Active Monitoring

```
[Traffic arrives at IPS]
          |
          ▼
[IPS evaluates traffic]
          |
          ├── Legitimate → forwarded to core switch → destination reached
          |
          └── Malicious → blocked at IPS → never reaches internal network
```

**Advantage:** Malicious traffic is stopped before it reaches any internal system.

**Risk:** The IPS is a single point of failure in the traffic path.

---

## Fail-Open vs. Fail-Closed

Because the IPS sits inline, what happens when it fails matters significantly:

| Fail Mode | Behavior on Failure | Network Impact | Security Impact |
|-----------|-------------------|----------------|----------------|
| **Fail-open** | Traffic continues to flow through the link | Network stays up | No security inspection — traffic passes uninspected |
| **Fail-closed** | Traffic stops — link is severed | Network goes down | No traffic passes at all |

Most networks prefer **fail-open** to maintain availability during a device failure — but this must be a deliberate decision, not an assumption. Always verify which mode your IPS uses by checking vendor documentation.

---

## Passive (Out-of-Band) Monitoring

In a **passive** configuration, the IPS sits outside the traffic path. Traffic flows normally between devices and the switch; the switch sends a *copy* of the traffic to the IPS for analysis.

### How Traffic is Copied

| Method | Description |
|--------|-------------|
| **Port mirror / SPAN** (Switch Port Analyzer) | Switch duplicates traffic on a specified port and sends the copy to the IPS |
| **Physical network tap** | Hardware device inserted into a cable that splits the signal, sending a copy to the IPS |

### Passive Monitoring Traffic Flow

```
[Traffic arrives at switch]
          |
          ├── Original → forwarded to destination (unaffected)
          |
          └── Copy → sent to IPS for evaluation
                    |
                    ├── Legitimate → IPS logs or ignores
                    |
                    └── Malicious → IPS alerts (cannot block — copy only)
```

**Advantage:** The IPS cannot cause network downtime — it is not in the traffic path.

**Limitation:** Malicious traffic still reaches its destination. The passive IPS alerts *after* the fact but cannot prevent the traffic from arriving.

---

## Inline vs. Passive: When to Use Each

| Consideration | Inline (Active) | Passive |
|--------------|----------------|---------|
| **Blocking capability** | Yes — stops malicious traffic | No — detection and alerting only |
| **Network downtime risk** | Yes — device failure can affect traffic | No — not in traffic path |
| **False positive risk** | High concern — legitimate traffic may be blocked | Lower concern — alerts only |
| **Typical use** | Production enforcement | Monitoring, testing, troubleshooting |
| **Equivalent to** | IPS | IDS (even if device is an IPS) |

> An IPS configured in passive mode functions effectively as an IDS — it has the same hardware but behaves as detection-only because it cannot intercept traffic to block it.

---

## Deployment Considerations

When an organization is concerned about:

- **Downtime from device failure** → Use passive monitoring or ensure fail-open mode
- **Overly aggressive blocking** → Start in passive mode; tune rules before going inline
- **Compliance requirements** → May require inline blocking for certain traffic types
- **New deployment** → Consider passive first to establish a traffic baseline, then transition to inline

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **IDS** | Detects and alerts on malicious traffic — cannot block |
| **IPS** | Detects and blocks malicious traffic in real time |
| **Inline monitoring** | IPS in the traffic path — can block but creates a single point of failure |
| **Fail-open** | IPS failure allows traffic to flow uninspected — network stays up |
| **Fail-closed** | IPS failure severs the network link — no traffic passes |
| **Passive monitoring** | IPS receives a copy of traffic — can alert but cannot block |
| **SPAN / port mirror** | Switch feature that copies traffic to the IPS port |
| **Passive IPS = IDS** | An IPS in passive mode has detection-only behavior regardless of its hardware capability |
