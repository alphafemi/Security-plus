# Data Classification & Sensitivity

## Overview

Not all data carries the same risk or requires the same level of protection. **Data classification** assigns sensitivity levels to different types of information so that appropriate access controls, storage requirements, and handling procedures can be applied. Understanding what data you have and how sensitive it is forms the foundation of any effective data protection strategy.

---

## Types of Sensitive Data

### Regulated Data

Some data is subject to external rules set by governments, industry bodies, or regulators — not just internal policy.

| Data Type | Regulatory Framework | Examples |
|-----------|---------------------|---------|
| **Payment card data** | PCI DSS (Payment Card Industry Data Security Standard) | Credit/debit card numbers, CVVs |
| **Health information (PHI)** | HIPAA (US), similar laws internationally | Medical records, diagnoses, health payments |
| **Personal data (EU residents)** | GDPR | Name, email, location data, identifiers |
| **Government records** | Varies by jurisdiction | Court records, tax filings, government ID numbers |

### Trade Secrets

**Proprietary data** that belongs exclusively to the organization — internal processes, formulas, strategies, source code, and other information that would provide competitive advantage to a rival if disclosed. This data would not commonly be found outside the organization.

### Intellectual Property

Creative or inventive work owned by the organization, protected through:
- **Copyrights** — literary, artistic, and software works
- **Trademarks** — brand names, logos, and identifiers
- **Patents** — inventions and processes

Unlike trade secrets, intellectual property may be publicly visible — but its use and reproduction is legally restricted.

### Legal Information

Legal records present a unique challenge: some must be public, while portions may contain sensitive personal information.

- Court records, judge and attorney information may be public
- Personally identifiable information (PII) within those records may be redacted or stored separately
- Different access systems may exist for public-facing records vs. sealed or sensitive portions

### Financial Information

- **Organizational financial data** — internal budgets, forecasts, M&A plans, investor information
- **Individual financial data** — bank account numbers, payment details, credit information

Both categories are sensitive but require different protection approaches and may be subject to different regulations.

---

## Data Formats

Data may be **human-readable** or **non-human-readable**, or a combination of both:

| Format | Description | Example |
|--------|-------------|---------|
| **Human-readable** | Directly interpretable by a person | Text documents, spreadsheets |
| **Non-human-readable** | Requires a machine or decoder | Barcodes, encoded binary data |
| **Hybrid** | Combines both | Barcode with human-readable numbers printed beneath it |

Non-human-readable formats may still contain sensitive information — the format alone does not determine sensitivity.

---

## Key Data Categories

### PII — Personally Identifiable Information

Information that can be used to identify a specific individual, directly or in combination with other data:

- Full name
- Date of birth
- Mother's maiden name
- Home address
- Email address
- Government ID numbers (SSN, passport, driver's license)
- Biometric data (fingerprints, facial recognition data)
- IP address (in some jurisdictions)

### PHI — Protected Health Information

Health-related information tied to a specific individual:

- Medical diagnoses and treatment history
- Prescription information
- Health care provider details
- Health insurance information
- Payments made for health care services

PHI is a subset of PII with additional regulatory protections (HIPAA in the US).

### Proprietary Data

Data that is unique to an organization and not publicly available:

- Internal processes and procedures
- Trade secrets and formulas
- Strategic plans and internal communications
- Unpublished research and development

---

## Data Classification Levels

Classification systems assign a sensitivity tier to data, which determines access controls, storage requirements, and handling procedures. Government and commercial classifications differ in terminology but share similar principles.

### Commercial Classification

| Level | Description | Access Control |
|-------|-------------|----------------|
| **Public / Unclassified** | Anyone may view this information | No restriction |
| **Sensitive** | Includes IP, PII, or PHI — requires baseline protection | Limited to authorized staff |
| **Confidential** | More sensitive than general data — restricted access | Need-to-know basis; often requires justification |
| **Private / Restricted** | Highly sensitive — significant harm if disclosed | Additional rights required; may require NDA |
| **Critical** | Must always be available — loss would severely impact operations | Maximum protection; strict availability controls |

### Government Classification (US)

| Level | Description |
|-------|-------------|
| **Unclassified** | Safe for public release |
| **Controlled Unclassified Information (CUI)** | Sensitive but not classified — requires protection |
| **Confidential** | Unauthorized disclosure could damage national security |
| **Secret** | Unauthorized disclosure could seriously damage national security |
| **Top Secret** | Unauthorized disclosure could cause exceptionally grave damage |

---

## Classification in Practice

Different classification levels drive different controls:

| Classification | Access Control | Storage | Transmission |
|---------------|---------------|---------|-------------|
| **Public** | None | Standard | Standard |
| **Sensitive** | Role-based access | Encrypted at rest | Encrypted in transit |
| **Confidential** | Need-to-know, logged | Encrypted, isolated | Encrypted, monitored |
| **Restricted** | Strict authorization, NDA required | Encrypted, separate system | Encrypted, controlled channels only |
| **Critical** | As above + availability controls | Redundant, encrypted | Encrypted + integrity monitoring |

---

## Data Ownership & Responsibility

| Role | Responsibility |
|------|---------------|
| **Data owner** | The business unit or individual accountable for a data set — defines classification and access policy |
| **Data custodian** | IT or security team responsible for implementing and maintaining the technical controls |
| **Data processor** | Third party that processes data on behalf of the owner (e.g., cloud provider, payroll service) |
| **Data subject** | The individual whose personal data is collected (especially relevant under GDPR) |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Data classification** | Assigns sensitivity tiers to data — drives access controls, storage, and handling requirements |
| **PII** | Information that can identify an individual — name, DOB, address, biometrics, government IDs |
| **PHI** | Health information tied to an individual — subject to HIPAA and similar regulations |
| **Proprietary data** | Organization-specific information — trade secrets, internal processes — not publicly available |
| **Regulated data** | Subject to external rules (PCI DSS, HIPAA, GDPR) — non-compliance carries legal and financial risk |
| **Public / Unclassified** | No access restrictions — safe for general distribution |
| **Confidential / Restricted** | Need-to-know access — may require NDA or explicit authorization |
| **Critical** | Must always be available — requires maximum availability and redundancy controls |
