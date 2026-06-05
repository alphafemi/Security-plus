# Email Security: SPF, DKIM & DMARC

## Overview

Email protocols were designed for message delivery — not security. Anyone can claim to send an email from any address. **SPF**, **DKIM**, and **DMARC** are DNS-based additions that provide authentication, integrity, and policy enforcement for email, making it possible to verify that an email genuinely originated from an authorized server.

---

## The Problem: Email Spoofing

Email headers can be freely forged. A message claiming to come from `james@professormesser.com` may have been sent by anyone. Without additional verification, there is no way to know if the claimed sender is real.

**Mail gateways** — the gatekeepers of incoming email for an organization — evaluate incoming messages before delivering them to inboxes. They check SPF, DKIM, and DMARC records to determine whether a message is legitimate or should be quarantined or discarded.

---

## SPF — Sender Policy Framework

**SPF** defines which mail servers are authorized to send email on behalf of a domain. It is stored as a **DNS TXT record** — publicly queryable by anyone.

### How SPF Works

```
[Sender: james@professormesser.com sends email]
          |
          ▼
[Recipient's mail gateway receives message]
          |
          ▼
[Gateway queries DNS: professormesser.com TXT records]
          |
          ▼
[SPF record found: "mailgun.org is authorized to send for professormesser.com"]
          |
          ▼
[Gateway checks: did this message originate from mailgun.org?]
          |
          ├── Yes → SPF passes; message is likely legitimate
          |
          └── No → SPF fails; message may be spoofed
```

### SPF DNS TXT Record Example

```
v=spf1 include:mailgun.org ~all
```

This record states:
- `v=spf1` — this is an SPF version 1 record
- `include:mailgun.org` — mailgun.org servers are authorized to send for this domain
- `~all` — all other sources should be treated with soft fail (quarantine/flag)

---

## DKIM — DomainKeys Identified Mail

**DKIM** adds a cryptographic digital signature to outbound email headers. The recipient can verify the signature using the sender's public key, which is published in a **DNS TXT record**.

### How DKIM Works

```
[Sender's mail server signs outbound message with DKIM private key]
          |
          ▼
[Message transmitted with DKIM-Signature header]
          |
          ▼
[Recipient's mail gateway finds DKIM-Signature in headers]
          |
          ▼
[Gateway queries sender's DNS for DKIM public key (TXT record)]
          |
          ▼
[Gateway validates signature using public key]
          |
          ├── Signature valid → message was not altered in transit; from authorized server
          |
          └── Signature invalid → message may be spoofed or tampered with
```

### Key Point

DKIM is a **server-level signature on the transport process** — it is not a visible signature on the message body. Recipients would need to inspect email headers to see it. The DKIM public key is stored in the domain's DNS TXT records, making it queryable by any receiving mail server.

---

## DMARC — Domain-Based Message Authentication, Reporting, and Conformance

**DMARC** builds on SPF and DKIM by specifying what the receiving server should **do** with messages that fail validation — and provides reporting back to the domain owner.

### What DMARC Controls

| Parameter | Options | Effect |
|-----------|---------|--------|
| **Policy** | `none`, `quarantine`, `reject` | What to do with messages that fail SPF/DKIM |
| **Reporting** | Email address for aggregate reports | Where compliance reports are sent |

### DMARC Policies

| Policy | Action on Failed Messages |
|--------|--------------------------|
| **none** | Take no action; only collect data (monitoring mode) |
| **quarantine** | Deliver to spam folder |
| **reject** | Do not deliver; discard the message entirely |

### DMARC DNS TXT Record Example

```
v=DMARC1; p=quarantine; rua=mailto:dmarc-reports@professormesser.com
```

This record states:
- `v=DMARC1` — this is a DMARC version 1 record
- `p=quarantine` — put failed messages in the spam/quarantine folder
- `rua=` — send aggregate compliance reports to this email address

### DMARC Reporting

DMARC reporting gives domain owners visibility into how their domain is being used — and abused:

- How many messages claiming to be from the domain were received
- How many passed SPF and DKIM validation
- How many failed (potential spoofing attempts)
- What happened to failed messages (quarantine, reject, none)

This data helps domain owners detect spoofing campaigns using their domain name.

---

## The Full Email Authentication Flow

```
[Incoming email: "From: james@professormesser.com"]
          |
          ▼
[Mail gateway checks SPF]
  → Does the sending server appear in the SPF record?
          |
          ▼
[Mail gateway checks DKIM]
  → Is the DKIM signature valid?
          |
          ▼
[Mail gateway checks DMARC]
  → What policy applies to failures?
          |
          ├── All pass → deliver to inbox
          ├── SPF/DKIM fail + policy=quarantine → spam folder
          └── SPF/DKIM fail + policy=reject → message discarded
          |
          ▼
[DMARC aggregate report sent to domain owner's reporting address]
```

---

## Where Records Are Added

All three records are added to the sending domain's DNS as **TXT records**:

| Record | DNS Entry Location | Content |
|--------|-------------------|---------|
| **SPF** | `professormesser.com` TXT | `v=spf1 include:mailgun.org ~all` |
| **DKIM** | `selector._domainkey.professormesser.com` TXT | Public key for signature validation |
| **DMARC** | `_dmarc.professormesser.com` TXT | `v=DMARC1; p=quarantine; rua=...` |

Because these are DNS records, they are publicly readable by any mail server on the internet — allowing any recipient to verify your domain's email policies.

---

## Mail Gateway Deployment

Mail gateways may be deployed:

| Location | Notes |
|----------|-------|
| **On-premises** | Typically placed in a screened subnet (DMZ) — needs to communicate with internet mail servers |
| **Cloud-based** | Third-party service handles all incoming mail filtering before delivering to internal mail servers |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Email spoofing** | Email protocols allow anyone to claim any sender address — SPF/DKIM/DMARC address this |
| **SPF** | DNS TXT record listing servers authorized to send email for a domain |
| **DKIM** | Cryptographic signature on email headers; recipient validates using domain's public key in DNS |
| **DMARC** | Policy for what to do with SPF/DKIM failures; enables compliance reporting to domain owner |
| **DMARC policies** | none (monitor only), quarantine (spam folder), reject (discard message) |
| **Mail gateway** | Checks SPF, DKIM, and DMARC before delivering email to users' inboxes |
| **All three use DNS TXT records** | Any mail server on the internet can query and verify your email authentication policies |
