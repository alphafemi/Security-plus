# Firewalls

## Overview

Firewalls control the flow of network traffic between two or more points. They are deployed at home, in enterprise networks, and as software within operating systems themselves. At their core, firewalls enforce a policy: what traffic is allowed to pass, and what is blocked.

Modern firewalls have evolved well beyond simple port-based filtering. Today's next-generation firewalls inspect the application layer, identify specific applications regardless of port number, and include integrated intrusion prevention, URL filtering, and more.

---

## Firewall Types

### Traditional Network Firewall (Layer 4)

Operates at **OSI Layer 4** — makes forwarding decisions based on:
- Source and destination IP addresses
- TCP or UDP port numbers
- Connection state (stateful inspection)

**Limitation:** Cannot distinguish between different applications that use the same port (e.g., legitimate HTTPS vs. malicious HTTPS). Cannot inspect what is *inside* the traffic.

---

### UTM — Unified Threat Management

A **UTM** bundles many security functions into a single appliance. Sometimes called a **web security gateway** or **all-in-one security appliance**.

| UTM Feature | Description |
|-------------|-------------|
| **Firewall** | Traditional port-based traffic control |
| **URL / content filtering** | Allow or block access to specific websites or content categories |
| **Anti-malware** | Scans traffic for known malicious software |
| **Spam filtering** | Blocks unwanted email at the network level |
| **IDS / IPS** | Detects and/or blocks known attack patterns |
| **VPN concentrator** | Provides secure remote access |
| **Bandwidth shaping / QoS** | Prioritizes traffic by application or protocol |
| **WAN connectivity** | May include CSU/DSU, routing, and switching |

**Limitation:** Most UTMs operate at Layer 4 — they look at port numbers, not application content. Enabling many features simultaneously can significantly degrade performance.

---

### NGFW — Next-Generation Firewall

A **Next-Generation Firewall** operates at **OSI Layer 7** (the application layer), performing **deep packet inspection** to understand what application is being used — regardless of port number.

Also called: application layer gateway, stateful multilayer inspection, or deep packet inspection firewall.

#### What NGFW Can Do

| Capability | Example |
|-----------|---------|
| **Application identification** | Recognize Microsoft SQL Server traffic regardless of port |
| **Granular application control** | Allow users to *view* Twitter but block *posting* to Twitter |
| **Content control** | Block YouTube video streaming while allowing other HTTPS |
| **URL categorization** | Block all sites in the "gambling" category |
| **Specific URL blocking** | Block espn.com and yahoo.com individually |
| **Integrated IPS** | Block traffic matching known vulnerability signatures |

#### Why Application Awareness Matters

Traditional firewalls rely on port numbers. An NGFW does not:

```
Traditional firewall sees:
  TCP port 443 → HTTPS → allowed (blanket rule)

NGFW sees:
  TCP port 443 → YouTube → blocked (per application policy)
  TCP port 443 → Corporate SaaS → allowed
  TCP port 443 → SQL injection payload → blocked (IPS)
```

Three completely different security outcomes — all on the same port.

---

### WAF — Web Application Firewall

A **Web Application Firewall** is specifically designed to analyze input into web-based applications and block malicious requests before they reach the application server.

| WAF Characteristic | Detail |
|-------------------|--------|
| **Traffic type** | HTTP and HTTPS web application traffic |
| **What it inspects** | The content of web requests — URLs, parameters, form inputs, headers |
| **What it blocks** | SQL injection, cross-site scripting (XSS), web scraping, malformed requests |
| **Position** | Sits in front of web servers — all inbound web traffic passes through it |
| **Complements** | Used alongside NGFW — each inspects different things |

#### WAF Log Example

A typical WAF log entry for a blocked SQL injection attempt:

```
Time:     2023-09-14 14:32:07
ID:       WF-10492
URL:      /index.cgi
Server:   demo1 (HTTP, port 80)
Client:   [client IP] [country]
Attack:   SQL Injection in Parameter
Action:   BLOCKED — standard security policy
```

The WAF identified that a parameter in the URL contained SQL injection syntax and blocked the request before it reached the application server.

#### WAF Compliance Requirement

**PCI DSS (Payment Card Industry Data Security Standard)** mandates the use of web application firewalls for organizations handling credit card data — recognizing that application-layer attacks against payment systems require application-layer defenses.

---

## Firewall Comparison

| Feature | Traditional | UTM | NGFW | WAF |
|---------|------------|-----|------|-----|
| **OSI layer** | Layer 4 | Layer 4 (mostly) | Layer 7 | Layer 7 |
| **Traffic basis** | IP/port | IP/port + features | Application + content | Web app input |
| **App awareness** | No | Limited | Yes | Yes (web only) |
| **IPS integration** | No | Yes (often) | Yes | No |
| **URL filtering** | No | Yes | Yes | Partial |
| **VPN support** | Sometimes | Yes | Yes | No |
| **Anti-malware** | No | Yes | Sometimes | No |
| **Best for** | Basic perimeter | SMB all-in-one | Enterprise application control | Web app protection |

---

## Additional Firewall Capabilities

Most firewalls — regardless of type — can also provide:

- **VPN termination** — acts as a VPN endpoint for remote users or site-to-site tunnels
- **Network Address Translation (NAT)** — translates between internal and external IP addresses
- **Routing** — acts as a Layer 3 device, routing traffic between network segments
- **Logging** — records all traffic flows, blocked connections, and policy matches for audit and forensics

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Traditional firewall** | Controls traffic by IP and port number — Layer 4 only |
| **UTM** | All-in-one appliance with many features; performance may degrade when all features are active |
| **NGFW** | Deep packet inspection at Layer 7 — identifies and controls traffic by application, not just port |
| **WAF** | Analyzes web application input to block SQL injection, XSS, and other web-layer attacks |
| **PCI DSS** | Mandates WAF deployment for organizations handling credit card data |
| **NGFW + WAF** | Complementary — NGFW controls network-layer application traffic; WAF specifically protects web apps |
