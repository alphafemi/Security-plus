# Security Monitoring & SIEM

## Overview

Security monitoring is the continuous observation of systems, networks, and applications to detect threats, anomalies, and policy violations in real time. The challenge is that monitoring data comes from dozens of diverse sources in different formats — which is why centralizing it in a **SIEM (Security Information and Event Manager)** is essential for effective security operations.

---

## What to Monitor

### Computing Resources

| Monitoring Target | What to Watch For |
|------------------|------------------|
| **Authentication events** | Failed logins, logins from unexpected countries, unusual hours |
| **Running services** | Unexpected services, unusual resource usage |
| **Software versions** | Outdated OS or applications that need patching |
| **Backup status** | Confirmed successful backups; failed backups |

### Applications

| Monitoring Target | What to Watch For |
|------------------|------------------|
| **Availability** | Application downtime or degraded performance |
| **Traffic volume** | Sudden spikes may indicate data exfiltration |
| **Error rates** | Unusual error patterns may indicate attack attempts |

### Network Infrastructure

| Monitoring Target | What to Watch For |
|------------------|------------------|
| **VPN connections** | Who is connecting — employees, vendors, guests |
| **Firewall logs** | Blocked connections, rule violations, spike in attack attempts |
| **IPS alerts** | Known attack pattern detection |
| **Data transfer volume** | Significantly above-normal outbound traffic |

---

## SIEM — Security Information and Event Manager

A **SIEM** consolidates log data from all monitoring sources into one central database, enabling unified reporting, correlation, and alerting.

### Data Sources

```
[Firewalls] ─────────────┐
[Switches / Routers] ────┤
[Servers] ───────────────┤──► [SIEM Database] ──► Reports
[VPN Concentrators] ─────┤                     ──► Correlation
[IDS / IPS] ─────────────┤                     ──► Alerts
[Authentication Systems] ┘
```

### What SIEM Enables

| Capability | Example |
|-----------|---------|
| **Unified reporting** | Single report covering authentication, network traffic, and application access |
| **Correlation** | Link VPN login event with subsequent data access — detect unusual post-login behavior |
| **Baseline comparison** | Establish normal data transfer volumes; alert when exceeded |
| **Historical investigation** | Review logs from months ago to trace attacker activity |
| **Compliance reporting** | Provide audit trails required by regulations |

---

## Continuous Vulnerability Scanning

Beyond SIEM, organizations run **continuous scanning** to maintain awareness of device status across the network:

| Scan Target | Information Gathered |
|-------------|---------------------|
| **Operating systems** | Versions installed; whether patching is current |
| **Device drivers** | Version information; known vulnerable drivers |
| **Installed applications** | What's on mobile devices, laptops, desktops |
| **Anomaly logs** | Unusual events already recorded on the device |

### Actionable Reports

The goal of continuous scanning is **actionable reporting** — not just collecting data, but producing outputs that drive specific next steps:

| Report Type | Action It Drives |
|------------|----------------|
| **Compliance status** | Which devices are fully patched; which are not |
| **OS inventory** | Which OS versions need updating |
| **Vulnerable systems** | How many systems are affected by a newly announced CVE |
| **Ad hoc / what-if** | How many systems will be affected when an OS reaches EOSL in 6 months |

---

## Dwell Time

The average attacker is inside a network for approximately **9 months** before being detected — a figure documented in IBM's 2022 security report.

```
[Attacker gains access]
          |
          ▼
[Attacker explores network — months pass]
  - Moves laterally between systems
  - Gains access to applications and data
  - Establishes persistence (backdoors, new accounts)
          |
          ▼ (~9 months later on average)
[Breach detected and contained]
```

### Implications

- Long-term log retention is essential — an investigation may need to trace activity from many months ago
- Regulatory requirements may mandate data retention for specific periods
- Real-time monitoring alone is insufficient — historical analysis is also necessary

---

## Real-Time Alerts

Security monitoring without alerting is passive — value comes from detecting events and notifying the right people immediately.

### Alert Triggers (Examples)

| Event | Alert Significance |
|-------|-------------------|
| **Spike in authentication failures** | Brute force, credential stuffing, or spraying attack |
| **Large outbound data transfer** | Potential data exfiltration |
| **Login from a new country** | Possible compromised credentials |
| **New service appearing on a server** | Potential malware installation |
| **IPS signature match** | Known attack pattern detected |

### Alert Delivery

| Channel | Use Case |
|---------|---------|
| **SMS / text** | Immediate notification for on-call responders |
| **Email** | More detail; suitable for medium-urgency events |
| **SOC dashboard** | Real-time display for security operations center staff |
| **Automated response** | Quarantine the affected system automatically |

### Automated Response: Quarantine

When a critical alert fires, the system can automatically quarantine the affected host:

```
[Alert: suspicious activity on workstation]
          |
          ▼ (automated response)
[Workstation isolated from network]
  - Cannot be reached by other systems
  - Cannot reach other systems
  - Attacker's access contained
  - Analyst investigates without attacker being able to spread
```

---

## Alert Tuning: False Positives and False Negatives

Poorly tuned alerting creates two failure modes:

| Problem | Description | Consequence |
|---------|-------------|-------------|
| **False positive** | Alert fires for a non-threatening event | Alert fatigue — responders start ignoring alerts |
| **False negative** | Real threat event produces no alert | Attacker activity goes undetected |

Good alert tuning finds the balance — sensitive enough to catch real threats, specific enough to avoid overwhelming responders with noise. This is typically an iterative process that improves over time as the baseline for "normal" behavior is better understood.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Security monitoring** | Continuous observation of systems, networks, and applications for threats and anomalies |
| **SIEM** | Centralizes log data from all sources — enables correlation, unified reporting, and alerting |
| **Actionable reports** | Reports designed to drive specific next steps — not just data collection |
| **Dwell time** | Average 9 months from breach to detection — long-term log retention is critical |
| **Real-time alerts** | Immediate notification of security events via SMS, email, or SOC dashboard |
| **Automated quarantine** | Isolates compromised systems instantly upon alert — limits lateral spread |
| **Alert tuning** | Ongoing process to minimize false positives (alert fatigue) and false negatives (missed threats) |
