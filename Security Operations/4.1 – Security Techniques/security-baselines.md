# Security Baselines

## Overview

A **security baseline** is a defined minimum set of security configurations that every system, application, or device in an organization must meet. When a new application instance is deployed, its security baseline must be deployed with it — and must remain in effect for the lifetime of that instance.

Security baselines cover operating system settings, application configurations, firewall rules, patch levels, file permissions, and any other security-relevant configuration across the entire application stack.

---

## Why Security Baselines Matter

Every component of an application instance has security-relevant settings:

| Component | Security Baseline Concerns |
|-----------|--------------------------|
| **Application** | Configuration settings, file permissions, authentication requirements |
| **Operating system** | Patch level, enabled services, user account policies, audit logging |
| **Firewall** | Rules governing what traffic is allowed in and out |
| **Network devices** | Authentication settings, management access restrictions |
| **Mobile devices** | MDM-enforced policies, encryption requirements |

Each time an application is deployed — whether to a new server, a cloud instance, or a new device — all of these security baselines must be deployed with it.

---

## Sources of Security Baselines

Organizations do not need to create security baselines from scratch. Authoritative sources provide well-researched starting points:

| Source | What They Provide |
|--------|------------------|
| **Application developer** | Configuration recommendations specific to the application |
| **OS manufacturer (e.g., Microsoft)** | OS-level security settings and group policy configurations |
| **Appliance manufacturer** | Security settings for purpose-built hardware devices |
| **CIS Benchmarks** | Center for Internet Security — vendor-neutral baselines for many platforms |
| **NIST guidelines** | US government security configuration guidelines |

### Microsoft Security Compliance Toolkit (SCT)

Microsoft provides a **Security Compliance Toolkit** for Windows operating systems and Windows Server that includes:

- Pre-configured security baselines based on Microsoft's recommendations
- Tools to deploy those baselines to managed systems
- Comparison tools to audit current settings against the baseline

Windows 10 alone has over **3,000 Group Policy settings** — only a subset apply to security, but the scale illustrates why automated tooling is essential.

---

## Deploying Security Baselines

Given the number and complexity of settings, manual deployment across hundreds or thousands of devices is not practical. Automated deployment is required.

### Deployment Methods

| Method | Best For |
|--------|---------|
| **Group Policy (Active Directory)** | Windows domain-joined systems — push settings centrally |
| **MDM (Mobile Device Management)** | Mobile devices and modern endpoints |
| **Microsoft Security Compliance Toolkit** | Windows OS and application baselines |
| **Infrastructure as Code (IaC)** | Cloud instances — security settings baked into deployment templates |
| **Configuration management tools** | Ansible, Puppet, Chef — cross-platform baseline enforcement |

Automated deployment ensures that every new system or instance receives the correct baseline configuration — without relying on manual steps that are easily missed.

---

## Maintaining Security Baselines

Baselines are not a one-time deployment. They require ongoing maintenance:

### When Baselines Must Be Updated

| Trigger | Action Required |
|---------|----------------|
| **New vulnerability discovered** | Update baseline to address the flaw |
| **Application update** | Review configuration changes introduced by the update |
| **New OS installation** | Deploy a new set of baselines appropriate to the new OS version |
| **Conflicting manufacturer recommendations** | Evaluate both baselines and select the more appropriate configuration |

### Baseline Conflict Resolution

Different manufacturers may have conflicting recommendations. For example:
- The OS manufacturer recommends disabling a service
- The application developer requires that service to be running

When conflicts arise, evaluate the security implications of each recommendation in the context of your specific environment and make a documented decision.

---

## Auditing Baselines

Deploying a baseline is not sufficient — it must also be verified and monitored:

```
[Deploy security baseline]
          |
          ▼
[Verify baseline is correctly applied]
          |
          ▼
[Monitor continuously for baseline drift]
  (settings changed by users, updates, or misconfigurations)
          |
          ├── Drift detected → remediate immediately
          |
          └── No drift → continue monitoring
```

**Baseline drift** occurs when settings deviate from the approved baseline over time — through updates, user changes, or configuration errors. Regular audits catch drift before attackers can exploit it.

---

## Security Baseline Lifecycle

```
[Identify applicable baselines]
  (OS, application, network, mobile)
          |
          ▼
[Compile baseline from authoritative sources]
  (manufacturer recommendations, CIS, NIST)
          |
          ▼
[Test baseline in non-production environment]
          |
          ▼
[Deploy to production via automated tooling]
          |
          ▼
[Audit continuously for drift and compliance]
          |
          ▼
[Update when triggered by vulnerabilities, updates, or new systems]
          |
          └──► (repeat)
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Security baseline** | Minimum required security configuration for every component of an application instance |
| **Must be deployed with every instance** | A new server, cloud instance, or device without its baseline is immediately non-compliant |
| **Use authoritative sources** | Microsoft, CIS Benchmarks, NIST, and vendor documentation provide starting points |
| **Automate deployment** | Group Policy, MDM, IaC, and configuration management tools are required at scale |
| **Microsoft SCT** | Free toolkit from Microsoft for deploying and auditing Windows security baselines |
| **Baseline drift** | Settings change over time — continuous monitoring and auditing are required |
| **Conflict resolution** | When manufacturer recommendations conflict, evaluate and document the chosen approach |
