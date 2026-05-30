# Network Appliances & Monitoring

## Overview

A secure network relies on more than firewalls and switches. Jump servers, proxy servers, load balancers, sensors, and SIEM platforms each play a distinct role in controlling access, managing traffic, and providing the visibility needed to detect and respond to threats.

---

## Jump Server (Bastion Host)

A **jump server** is a hardened, access-controlled device inside the network that serves as the single gateway for external administrators to reach internal systems.

### How It Works

```
[External Administrator]
          |
          | (authenticate to jump server)
          ▼
[Jump Server — hardened, monitored, access-controlled]
          |
          | (SSH or RDP to internal system)
          ▼
[Internal Web Server / Database / Device]
```

Administrators never connect directly to internal systems from the outside — they connect to the jump server first, then pivot internally from there.

### Security Requirements

- Must be **heavily hardened** — if compromised, every internal system it can reach is at risk
- Access should require **MFA** and be restricted to authorized administrators only
- All sessions should be **logged and monitored**
- Should have **minimal services** running — only what is needed for remote administration

---

## Proxy Servers

A **proxy** sits between clients and servers, making requests on behalf of one party. Proxies can filter content, cache responses, scan for malware, and hide the identity of internal systems.

### Proxy Types by Direction

| Type | Direction | Use Case |
|------|-----------|---------|
| **Forward proxy** | Outbound — internal users → internet | URL filtering, content scanning, caching, bandwidth control |
| **Reverse proxy** | Inbound — internet → internal servers | Security in front of web servers, caching, load distribution |
| **Open proxy** | Any direction — publicly accessible | Bypassing security controls; significant security risk |

### Forward Proxy Flow

```
[Internal User]
      |
      | (request to proxy)
      ▼
[Forward Proxy]
  - URL filtering
  - Content scanning
  - Caching
      |
      | (proxy makes request to internet on user's behalf)
      ▼
[Internet / External Site]
      |
      ▼ (response back to proxy → evaluated → sent to user)
[Internal User receives response]
```

### Reverse Proxy Flow

```
[Internet User]
      |
      | (connects to proxy, not directly to web server)
      ▼
[Reverse Proxy]
  - Malicious traffic filtered
  - Cache served if available
  - SSL termination
      |
      | (proxy forwards to web server)
      ▼
[Internal Web Server]
```

The web server is never directly exposed to the internet — all traffic passes through the proxy first.

### Explicit vs. Transparent Proxies

| Type | Configuration | User Awareness |
|------|-------------|----------------|
| **Explicit** | Client must be configured with proxy IP/hostname | User knows the proxy exists |
| **Transparent** | No client configuration needed — proxy intercepts automatically | User has no idea the proxy is in place |

### Proxy Features

| Feature | Description |
|---------|-------------|
| **Caching** | Stores responses to common requests; subsequent requests served from cache without going to the internet |
| **URL filtering** | Blocks access to specific websites or categories |
| **Content scanning** | Analyzes traffic for malware, exploits, or policy violations |
| **Protocol support** | Can proxy HTTP, HTTPS, FTP, and other protocols |
| **NAT** | A basic form of proxy — translates between internal and external IP addresses |

### Open Proxy Risk

An **open proxy** is publicly accessible to anyone on the internet. Using one means:

- Routing traffic through an **unknown third party**
- That third party can **read, modify, or inject content** into the traffic
- May be used to **circumvent organizational security controls**
- Often blocked by security-conscious organizations for exactly these reasons

---

## Load Balancers

A **load balancer** distributes incoming traffic across multiple servers to improve performance, ensure availability, and provide fault tolerance.

### Active/Active Configuration

All connected servers are active simultaneously. The load balancer distributes traffic across all of them.

```
[Users]
   |
[Load Balancer]
   ├──► [Server A] ← active
   ├──► [Server B] ← active
   ├──► [Server C] ← active
   └──► [Server D] ← active
```

If any server fails, the load balancer detects it and redistributes that server's share of traffic to the remaining active servers — typically without users noticing.

### Active/Passive Configuration

Some servers are active; others are on standby. Standby servers activate only when an active server fails.

```
[Load Balancer]
   ├──► [Server A] ← active ✓
   ├──► [Server B] ← active ✓
   ├──► [Server C] ← standby (passive)
   └──► [Server D] ← standby (passive)

[Server A fails]
   ├──► [Server B] ← active ✓
   ├──► [Server C] ← now active ✓ (promoted from standby)
   └──► [Server D] ← standby (passive)
```

### Load Balancer Capabilities

| Feature | Description |
|---------|-------------|
| **Traffic distribution** | Spreads load evenly across all active servers |
| **Fault tolerance** | Detects failed servers and redirects traffic automatically |
| **TCP offloading** | Maintains persistent TCP connections to servers — eliminates per-user TCP handshake overhead |
| **SSL/TLS offloading** | Handles encryption/decryption centrally, using purpose-built hardware — servers receive cleartext |
| **Caching** | Serves cached responses for repeated identical requests |
| **Content switching** | Routes specific request types to designated servers (e.g., image requests → image servers) |
| **Prioritization** | Gives certain applications or protocols higher processing priority |

---

## Sensors & Collectors

Individual security devices — switches, routers, firewalls, IPS, authentication systems, web servers — each generate valuable data. **Sensors** collect this data; **collectors** aggregate it.

### Data Sources

| Source | Data Provided |
|--------|-------------|
| **Switches / routers** | Traffic statistics, port utilization, MAC address tables |
| **Firewalls** | Traffic flows, blocked connections, policy match logs |
| **IPS/IDS** | Alert logs, detected attack patterns, blocked traffic |
| **Authentication systems** | Login success/failure, account lockouts, privilege changes |
| **Web servers** | Access logs, error logs, request patterns |
| **Dedicated sensors** | Purpose-built devices that passively monitor network traffic |

### SIEM — Security Information and Event Manager

A **SIEM** is the central aggregator and analysis platform for all sensor and log data across the environment.

```
[Switches] ─────────────┐
[Firewalls] ────────────┤
[IPS / IDS] ────────────┤──► [SIEM Collector / Database]
[Auth Systems] ─────────┤         |
[Web Servers] ──────────┤         ▼
[Dedicated Sensors] ────┘    [Correlation Engine]
                                   |
                         ┌─────────┴─────────┐
                         ▼                   ▼
                    [Alerts]            [Reports]
                  Real-time        Historical analysis,
                 notification      compliance, forensics
```

### SIEM Capabilities

| Capability | Description |
|-----------|-------------|
| **Consolidation** | All logs from all systems in one searchable database |
| **Correlation** | Connects related events across different systems (e.g., failed login → port scan → lateral movement) |
| **Alerting** | Real-time notification when suspicious patterns are detected |
| **Reporting** | Historical reports for compliance, audits, and incident investigation |
| **Forensic analysis** | Enables post-incident reconstruction of attacker activity |

---

## Summary

| Device | Primary Purpose | Key Security Benefit |
|--------|----------------|---------------------|
| **Jump server** | Controlled admin access gateway | Single, hardened path to internal systems; all admin access logged |
| **Forward proxy** | Controls outbound traffic | URL filtering, content scanning, caching |
| **Reverse proxy** | Protects inbound access to web servers | Filters malicious requests before they reach the server |
| **Open proxy** | Circumvents security controls | **Risk** — unknown third party can intercept and modify traffic |
| **Load balancer** | Distributes traffic across servers | Fault tolerance; SSL offload; content switching |
| **Sensor** | Collects traffic and event data | Provides raw visibility into network activity |
| **SIEM** | Aggregates and correlates all security data | Centralized detection, alerting, reporting, and forensics |
