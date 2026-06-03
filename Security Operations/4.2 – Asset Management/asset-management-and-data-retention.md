# Asset Management, Data Sanitization & Retention

## Overview

Managing the full lifecycle of hardware and software assets — from acquisition through decommissioning — is a foundational IT security responsibility. This includes tracking every asset, properly sanitizing storage media before reuse or disposal, and retaining data for the legally and operationally required period.

---

## Procurement & Acquisition

The formal acquisition process typically involves multiple departments and approval levels:

```
[End user identifies need]
          |
          ▼
[IT evaluates technical requirements]
          |
          ▼
[Purchasing department checks budget and approvals]
          |
          ▼
[Negotiation with supplier — price, licensing, contract terms]
          |
          ▼
[Purchase order issued → goods/services delivered]
          |
          ▼
[Invoice received → payment per agreed terms (net 30, net 60, etc.)]
          |
          ▼
[Asset added to tracking system]
```

Formal procurement processes create an auditable record and ensure that organizational security requirements (vendor vetting, licensing compliance) are part of every purchase.

---

## Asset Tracking

An **asset tracking system** manages the full inventory of hardware and software assets across their entire lifecycle.

### What Asset Tracking Records

| Data Point | Description |
|-----------|-------------|
| **Owner / assigned user** | Who is responsible for this asset |
| **Asset type** | Laptop, desktop, server, mobile device, network device, etc. |
| **Hardware vs. software** | Determines accounting and tax treatment |
| **Make, model, serial number** | Identifies the specific device |
| **Component inventory** | CPU, RAM, storage, peripherals within a device |
| **Asset tag / barcode** | Physical label for identification and tracking |
| **Location** | Where the device is physically located |

### Hardware vs. Software (Tax Implications)

| Asset Type | Accounting Classification | Tax Treatment |
|-----------|--------------------------|--------------|
| **Hardware** | Capital expenditure (CapEx) | Subject to depreciation |
| **Software** | Operating expenditure (OpEx) | Expensed immediately; no depreciation |

### Benefits of Asset Tracking

- **Help desk efficiency** — technicians can pre-populate tickets with device make, model, and configuration
- **Security audits** — inventory scans reveal unknown or unmanaged devices on the network
- **Lifecycle management** — track when devices approach EOL/EOSL
- **Loss/theft response** — asset tags help identify recovered stolen equipment

---

## Asset Tags

An **asset tag** is a physical label — barcode, QR code, or RFID — affixed to a device that links it to its record in the asset tracking system.

Benefits:
- Enables rapid identification during audits
- Acts as a deterrent to theft (clearly marks equipment as organizational property)
- Aids recovery of lost or stolen devices

---

## Data Sanitization

When a device is retired, reused, or disposed of, the data on its storage must be properly handled. The appropriate sanitization method depends on the device's next destination.

### Sanitization Decision Tree

```
[Device is being retired]
          |
          ├── Reusing internally for another employee?
          │         → Secure delete / overwrite (data gone; drive reusable)
          │
          ├── Donating or selling externally?
          │         → Secure delete or physical destruction
          │
          └── Disposing of entirely?
                    → Physical destruction or degaussing
```

### Sanitization Methods

| Method | Description | Drive Reusable? |
|--------|-------------|-----------------|
| **Secure delete / overwrite** | Overwrites data with zeros or random data — software-based; verified approaches prevent recovery | Yes |
| **Degaussing** | Strong electromagnetic field erases all magnetic data; renders hard drives unusable | No (HDDs) |
| **Physical shredding** | Industrial shredder pulverizes the drive | No |
| **Drilling / hammering** | Physically damages platters — DIY method for small quantities | No |
| **Incineration** | Complete destruction — absolute guarantee | No |

### Third-Party Drive Destruction

Organizations with large numbers of drives to dispose of often contract with specialized destruction services. Requirements:

- Third party must **document and confirm** destruction of every drive
- A **certificate of destruction** is issued, confirming all drives have been permanently destroyed
- The certificate provides an auditable record for compliance purposes

> A certificate of destruction is required by many compliance frameworks when disposing of media that contained regulated data (PII, PHI, financial records).

---

## Data Retention

**Data retention** is the policy and practice of keeping data for a specified period — either because it is legally required or because it serves operational needs.

### Why Data Retention Is Required

| Reason | Example |
|--------|---------|
| **Legal / regulatory mandate** | Emails retained for 7 years; financial records for a legally defined period |
| **Industry compliance** | PCI DSS, HIPAA, GDPR all specify retention periods for different data types |
| **Litigation hold** | Data that may be relevant to legal proceedings must be preserved |
| **Operational need** | Backups retained to support disaster recovery and accidental deletion recovery |

### What Retention Policies Cover

| Data Type | Considerations |
|-----------|---------------|
| **Original data** | The primary source — must be retained in its original form |
| **Copies** | Any duplicated versions that exist across systems |
| **Backups** | Backup copies must also fall under retention policy |
| **Email** | Often subject to the longest and most specific retention requirements |
| **Financial records** | Tax and audit requirements vary by jurisdiction |

### Retention Policy Components

A complete data retention policy defines:

- **What data** is covered
- **How long** it must be retained
- **Where** it must be stored (on-site, off-site, cloud)
- **Who** is responsible for managing it
- **How** it is deleted when retention period ends (using appropriate sanitization)

---

## Asset Lifecycle Summary

```
[Procurement] → formal approval, negotiation, purchase
      |
      ▼
[Receipt & Asset Tagging] → added to tracking system, tagged
      |
      ▼
[Active Use] → monitored, patched, supported
      |
      ▼
[Redeployment / Transfer] → reassigned to new user; secure delete if needed
      |
      ▼
[End of Life] → replaced; data sanitized
      |
      ▼
[Disposal] → physical destruction, degaussing, or certified third-party destruction
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Asset tracking** | Full lifecycle inventory of all hardware and software — owner, type, components, location |
| **Asset tag** | Physical identifier linking device to tracking system record |
| **Secure delete** | Software-based overwrite — drive reusable after; sufficient for internal redeployment |
| **Degaussing** | Electromagnetic erasure — renders HDDs permanently unusable |
| **Physical destruction** | Drilling, shredding, or incineration — absolute guarantee of data destruction |
| **Certificate of destruction** | Third-party confirmation that drives were destroyed — required for compliance |
| **Data retention** | Legal, regulatory, or operational requirement to keep data for a defined period |
| **Retention policy** | Defines what data, how long, where stored, and how destroyed when the period ends |
