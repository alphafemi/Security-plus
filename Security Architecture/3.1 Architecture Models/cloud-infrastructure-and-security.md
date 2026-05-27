# Cloud Infrastructure & Security

## Overview

Cloud computing introduces new architectural models, shared responsibilities, and security challenges that don't exist in traditional on-premises environments. Understanding who is responsible for what, how multi-cloud complexity affects security, and how modern cloud architectures (serverless, microservices) change the security model is essential for any organization running workloads in the cloud.

---

## Shared Responsibility Model

In a public cloud, security responsibility is **divided** between the cloud provider and the customer. The exact division depends on the service model being used.

### Responsibility by Service Model

| Layer | SaaS | PaaS | IaaS | On-Premises |
|-------|------|------|------|------------|
| **Application** | Provider | Customer | Customer | Customer |
| **Runtime / middleware** | Provider | Provider | Customer | Customer |
| **Operating system** | Provider | Provider | Customer | Customer |
| **Virtualization** | Provider | Provider | Provider | Customer |
| **Physical hardware** | Provider | Provider | Provider | Customer |
| **Accounts & identities** | Customer | Customer | Customer | Customer |

> **Key principle:** Regardless of service model, **the customer is always responsible for their own accounts, identities, and data**. The cloud provider secures the infrastructure; the customer secures what they put on it.

### What This Means in Practice

- Review your cloud provider's responsibility matrix before deploying any workload
- Contracts may modify the default matrix — understand what your specific agreement covers
- Do not assume the provider handles something unless it is explicitly documented

---

## Hybrid & Multi-Cloud Environments

Many organizations use multiple cloud providers simultaneously — a **hybrid cloud** model. This adds flexibility but introduces significant security complexity.

### Multi-Cloud Security Challenges

| Challenge | Description |
|-----------|-------------|
| **No cross-provider communication** | Cloud providers don't natively talk to each other — configurations must be managed separately on each |
| **Configuration mismatch** | Authentication, firewall rules, and server settings may be configured differently across providers — creating gaps |
| **Log format differences** | Each provider uses different log formats and terminology — consolidating them for a unified security view is difficult |
| **Data in transit** | Data flowing between cloud providers traverses the public internet — must be encrypted end-to-end |

### Mitigations

- Use a **SIEM** that can ingest and normalize logs from multiple cloud providers
- Apply consistent security policies using cloud-agnostic infrastructure as code
- Enforce encryption for all inter-cloud data transfers
- Implement a **vendor risk management policy** to govern third-party cloud services and applications

---

## Third-Party Cloud Components

Most cloud deployments include third-party components — firewalls, monitoring tools, databases, or SaaS integrations. Each third party extends the attack surface.

### Management Requirements

| Area | Consideration |
|------|--------------|
| **Vendor risk management** | Formal policy to assess and manage security risks from third-party products |
| **Incident response** | Third parties must be included in IR planning — their systems may be involved in a breach |
| **Continuous monitoring** | Third-party availability and security posture should be monitored, not assumed |

---

## Infrastructure as Code (IaC)

**Infrastructure as Code** defines cloud infrastructure — servers, web servers, databases, network configurations — as code rather than manually configured hardware. The infrastructure is reproducible, version-controlled, and deployable anywhere.

### Benefits

- **Consistency** — the same configuration is deployed every time, eliminating manual drift
- **Portability** — the same code can build the infrastructure on any cloud provider
- **Speed** — entire environments can be spun up or torn down in minutes
- **Auditability** — infrastructure changes are tracked in version control

### Security Consideration

IaC files themselves must be secured — a misconfigured IaC template deployed at scale replicates the misconfiguration to every instance. Code review and security scanning of IaC templates is essential.

---

## Serverless Architecture

**Serverless** removes the concept of servers from the developer's view. Instead of running a monolithic application on a server, functionality is broken into **individual functions** that run only when called — in whatever OS environment is appropriate.

### How It Works

```
[Client / Application]
          |
          ▼
[Function: Authentication]  ← runs only when login is needed
[Function: Data retrieval]  ← runs only when data is requested
[Function: Payment processing] ← runs only for transactions
          |
          ▼
[Each function spins up, runs, and is removed when no longer needed]
```

### Security Implications

| Property | Security Impact |
|----------|----------------|
| **No persistent OS** | Eliminates OS-level attack surface; no persistent foothold for attackers |
| **Short-lived execution** | Functions run and disappear — malware cannot persist in the traditional sense |
| **Provider-managed runtime** | The cloud provider patches and maintains the underlying execution environment |
| **Fine-grained permissions** | Each function can be granted only the permissions it specifically needs |

---

## Microservices Architecture

**Microservices** decompose a traditional monolithic application into **independent, specialized services** that communicate through APIs.

### Monolithic vs. Microservices

| Property | Monolithic | Microservices |
|----------|-----------|---------------|
| **Structure** | One large executable handles all functions | Many small, independent services |
| **Deployment** | Full application update required for any change | Individual services updated independently |
| **Scalability** | Scale the entire application | Scale only the services under load |
| **Resilience** | One failure can affect the entire app | A failed service doesn't bring down others |
| **Security scope** | One security model for the entire app | Each service can have its own security controls |

### Microservices Security

```
[Client]
   |
   ▼
[API Gateway]  ← single entry point; handles authentication and routing
   |
   ├──► [Auth Service]     ← handles login; strict credential controls
   ├──► [Data Service]     ← reads/writes database; least-privilege DB access
   ├──► [Payment Service]  ← PCI-compliant controls for card processing
   └──► [Notification Service] ← outbound only; limited network permissions
```

Each microservice enforces only the security controls appropriate to its function — the payment service has stricter controls than the notification service, for example.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Shared responsibility** | Cloud providers secure the infrastructure; customers are always responsible for accounts, identities, and data |
| **Hybrid/multi-cloud** | Multiple providers increase flexibility but require careful configuration management and log consolidation |
| **Third-party risk** | Every external component extends the attack surface — formal vendor risk management is required |
| **Infrastructure as Code** | Reproducible, auditable infrastructure — but IaC templates must themselves be secured and reviewed |
| **Serverless** | Functions run on-demand with no persistent OS — reduces persistent attack surface; provider manages the runtime |
| **Microservices** | Independent services communicate via API — enables per-service security controls and isolated blast radius |
| **API gateway** | Single controlled entry point for microservice architectures — handles authentication and routing |
