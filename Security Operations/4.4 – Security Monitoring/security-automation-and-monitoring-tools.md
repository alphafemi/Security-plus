# Security Automation & Network Monitoring Tools

## Overview

Managing security across hundreds or thousands of diverse devices requires automation and standardization. This page covers the tools and protocols that enable automated compliance checking, data loss prevention, and network-level monitoring — from SCAP and CIS Benchmarks through SNMP and NetFlow.

---

## SCAP — Security Content Automation Protocol

**SCAP** (pronounced "ess-cap") is a protocol maintained by NIST that creates a common language for describing vulnerabilities and security configurations across diverse security tools.

### The Problem SCAP Solves

A next-generation firewall, an IPS, and a vulnerability scanner might all detect the same vulnerability — but describe it using different names, terms, and identifiers. Without a common language, correlating these findings is manual and error-prone.

With SCAP, all three tools use the same identifier for the same vulnerability.

### What SCAP Enables

```
[NGFW detects vulnerability] ────────┐
[IPS detects same vulnerability] ────┼──► [SCAP: all describe same CVE ID]
[Vulnerability scanner finds it] ────┘           |
                                                  ▼
                                    [Management system receives unified report]
                                                  |
                                                  ▼
                                    [Automated patch deployment — no human intervention]
```

### More Information

SCAP resources are available at: **scap.nist.gov**

---

## CIS Benchmarks

The **Center for Internet Security (CIS)** publishes security benchmarks — curated sets of security configuration best practices for specific operating systems, applications, and cloud platforms.

**Available at:** cissecurity.org

### Example Benchmark Controls (Mobile Device)

| Control | Action |
|---------|--------|
| Disable screenshots | Prevents sensitive screen content from being captured |
| Disable screen recordings | Prevents video capture of screen activity |
| Prevent voice calls when locked | Limits attack surface when device is unattended |
| Force encrypted backups | Ensures backup data is protected |

Applying a benchmark configures a system to the highest reasonable security posture from initial deployment — rather than relying on administrators to manually identify and apply every setting.

---

## Compliance Checking: Agent vs. Agentless

Organizations continuously verify that devices meet security baselines. Two approaches exist:

### Agent-Based Checking

A persistent software agent is installed on every managed device.

| Property | Detail |
|----------|--------|
| **Always on** | Continuously monitors compliance — catches changes immediately |
| **Requires setup** | Agent must be installed before monitoring can begin |
| **Ongoing maintenance** | Agent software and signature databases must be kept updated |
| **Best for** | Managed corporate devices with consistent software deployment |

### Agentless Checking

No persistent installation — the check runs at login or VPN connection, verifies compliance, then removes itself.

| Property | Detail |
|----------|--------|
| **No installation required** | Executes in memory; removes itself when done |
| **No ongoing maintenance** | No agent to update |
| **Not always-on** | Only checks when triggered (login, VPN connect) |
| **Best for** | BYOD devices, guest access, or systems that can't run persistent agents |

---

## Data Loss Prevention (DLP)

**DLP** monitors network traffic and endpoint activity to detect and block unauthorized transmission of sensitive data.

### What DLP Can Detect and Block

| Data Type | Examples |
|-----------|---------|
| **PII** | Social Security numbers, passport numbers |
| **Financial data** | Credit card numbers, bank account information |
| **Health information** | Medical records, diagnoses, PHI |
| **Intellectual property** | Source code, proprietary documents |

### DLP Deployment Points

| Location | Coverage |
|----------|---------|
| **Endpoint (DLP agent)** | Monitors data on the device — copies, transfers, prints |
| **Network (inline appliance)** | Monitors traffic crossing the network boundary |
| **Cloud-based DLP** | Monitors data in cloud storage and SaaS applications |

DLP provides real-time blocking — if an employee (or attacker) attempts to transfer sensitive data, DLP can intercept and block it before it leaves the organization.

---

## SNMP — Simple Network Management Protocol

**SNMP** collects low-level metrics from network devices — routers, switches, servers, firewalls — and reports them to a central management station.

### How SNMP Works

| Component | Role |
|-----------|------|
| **SNMP agent** | Software running on the monitored device; collects metrics |
| **MIB** (Management Information Base) | Database of available metrics on the device |
| **OID** (Object Identifier) | Numeric identifier for a specific metric |
| **Network Management Station (NMS)** | Polls devices and collects responses; creates graphs and reports |

### SNMP Ports

| Port | Function |
|------|---------|
| **UDP 161** | NMS polls device; device responds with metric value |
| **UDP 162** | Device sends **SNMP trap** (proactive alert) to NMS |

### Polling vs. Traps

**Polling:** The NMS asks a device for a metric value every N minutes. No poll = no data for that interval.

**Traps:** The device proactively sends an alert when a condition is met — without waiting to be polled.

```
Example trap: "Send alert when CRC errors increase by 5"
[CRC errors increase by 5 on interface]
          |
          ▼ (device initiates — no waiting for poll)
[SNMP trap sent to NMS over UDP 162]
          |
          ▼
[NMS fires alert, sends SMS, initiates automation]
```

Traps are more responsive than polling for time-sensitive alerts.

---

## NetFlow

**NetFlow** provides application-level traffic flow analysis — going beyond SNMP's device-level metrics to show what applications and endpoints are communicating across the network.

### NetFlow Architecture

```
[Network traffic flows through switch/router]
          |
          ├──► [NetFlow probe] ← may be built into device or external
          |          |
          |          ▼
          |    [NetFlow collector] ← aggregates flow data
          |          |
          |          ▼
          |    [Reports / dashboards]
          |
          └──► [Normal traffic path continues]
```

### What NetFlow Reports

| Report | Value |
|--------|-------|
| **Top 10 conversations** | Which endpoints are talking to each other and how much |
| **Top 10 endpoints** | Busiest sources/destinations by IP or hostname |
| **Application traffic breakdown** | Which applications are generating what volume |
| **Traffic anomalies** | Unusual flow patterns that may indicate attack or exfiltration |

### SNMP vs. NetFlow

| Property | SNMP | NetFlow |
|----------|------|---------|
| **Focus** | Device metrics (utilization, errors) | Traffic flows (who, what, how much) |
| **Granularity** | Interface-level | Application and conversation level |
| **Alerting** | Traps for threshold events | Flow anomaly detection |
| **Best for** | Device health monitoring | Application and security traffic analysis |

---

## Summary

| Tool / Protocol | Purpose | Key Takeaway |
|----------------|---------|-------------|
| **SCAP** | Common language for security vulnerabilities | Enables cross-tool correlation and automated patching |
| **CIS Benchmarks** | Security configuration best practices | Pre-built secure configurations for OS, apps, and cloud |
| **Agent-based compliance** | Always-on compliance monitoring | Continuous; requires installation and maintenance |
| **Agentless compliance** | On-demand compliance check at login | No installation; not always-on |
| **DLP** | Detects and blocks sensitive data transfers | Endpoint, network, and cloud deployment options |
| **SNMP** | Device metrics collection and alerting | Polling (UDP 161) + traps (UDP 162) |
| **NetFlow** | Application traffic flow analysis | Shows who is talking to whom and how much |
