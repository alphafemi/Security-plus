# Password Attacks

## Overview

Passwords are among the most targeted credentials in any system. Attackers use a range of techniques — from trying the most common passwords across many accounts to exhaustively hashing every possible character combination offline. Understanding how these attacks work explains why strong password policies, proper storage, and account lockout controls are essential.

---

## Plain Text Password Storage

The most catastrophic password vulnerability is storing them as **plain text**. If an attacker gains access to the file or database where credentials are stored, they immediately have every username and password — no cracking required.

| Storage Method | If Database Is Stolen |
|---------------|----------------------|
| **Plain text** | Every password is immediately readable |
| **Hashed** | Attacker must crack each hash — time-consuming and computationally expensive |
| **Salted hash** | Cracking is further slowed; rainbow tables are defeated |

> If an application stores passwords in plain text, **stop using it**. This requires a full application rewrite to fix — it is not a configuration change.

---

## Password Hashing

All passwords should be stored as **hashes** — a fixed-length fingerprint of the original password produced by a one-way algorithm. The original password cannot be reconstructed from the hash alone.

### Key Properties for Password Storage

- **One-way** — the hash cannot be reversed to reveal the password
- **Deterministic** — the same password always produces the same hash
- **Sensitive** — a one-character difference in password produces a completely different hash

**Example (SHA-256):**

| Password | Stored Hash |
|----------|------------|
| `123456` | `8d969eef6ecad3...` |
| `1234567` | `9C9064C59F1FFA...` |

Although the passwords differ by only one character, the hashes are completely different — making it impossible to infer one password from another's hash.

---

## Password Spraying

A **password spraying** attack avoids account lockout by trying a small number of the most common passwords across a **large number of accounts** — rather than many passwords against one account.

### The Most Common Passwords

| Rank | Password |
|------|---------|
| 1 | `123456` |
| 2 | `123456789` |
| 3 | `qwerty` |
| 4 | `password` |
| 5 | `1234567` |

### How Spraying Works

```
[Attacker tries "123456" on Account A]  → fails → move on
[Attacker tries "123456" on Account B]  → fails → move on
[Attacker tries "123456" on Account C]  → SUCCESS
...
[Repeat with "password" across all accounts]
```

By limiting attempts to 2–3 per account, the attacker:
- **Avoids triggering lockout policies** — most systems lock after 5–10 failed attempts
- **Generates no alarms** — a small number of failures per account looks like normal user error
- **Operates at scale** — even a 1% success rate against thousands of accounts yields many compromised credentials

> Password spraying is effective precisely because it is patient and quiet. It exploits the fact that a significant percentage of users choose the same weak passwords.

---

## Brute Force Attacks

A **brute force** attack exhaustively tries every possible password combination until one matches. Unlike spraying, it targets specific accounts or hashes rather than spreading across many accounts.

### Online Brute Force

Attempted directly against a live login system.

- **Slow** — network latency and rate limiting restrict attempt frequency
- **Risky** — account lockout policies may trigger, alerting administrators
- **Limited** — typically only feasible against accounts without lockout protection

### Offline Brute Force

The attacker first obtains the password hash file (e.g., from a database breach), then cracks it locally with no connection to the target system.

```
[Attacker obtains password hash file]
          |
          ▼
[No lockout risk — all processing is local]
          |
          ▼
[Try "aaaaa" → hash → compare to target hash → no match]
[Try "aaaab" → hash → compare to target hash → no match]
[Try "aaaac" → hash → compare to target hash → no match]
...
[Try "password" → hash → compare to target hash → MATCH]
          |
          ▼
[Password recovered: "password"]
```

A typical password hash file contains usernames, account metadata, and the hashed password for each account. With this file, the attacker can work offline indefinitely — targeting all accounts simultaneously with no time pressure.

### What Determines Brute Force Speed

| Factor | Effect |
|--------|--------|
| **Hash algorithm strength** | Slower algorithms (bcrypt, scrypt) dramatically increase crack time |
| **Password length** | Each additional character multiplies the search space exponentially |
| **Character set** | Including uppercase, numbers, and symbols increases combinations |
| **Attacker's compute resources** | GPUs and botnets can test billions of hashes per second against weak algorithms |

---

## Spraying vs. Brute Force

| Property | Password Spraying | Brute Force |
|----------|-----------------|-------------|
| **Passwords tried per account** | Very few (2–3) | All possible combinations |
| **Accounts targeted** | Many simultaneously | Typically one or a few |
| **Lockout risk** | Very low | High (online) / None (offline) |
| **Goal** | Find accounts with weak passwords | Crack a specific password |
| **Detection difficulty** | Hard — looks like normal failures | Easier — generates high failure volume |

---

## Defenses

| Defense | Protects Against |
|---------|-----------------|
| **Never store passwords as plain text** | Eliminates immediate credential exposure from database breaches |
| **Use strong hashing algorithms (bcrypt, Argon2)** | Dramatically slows offline brute force |
| **Salt every password hash** | Defeats rainbow tables; ensures identical passwords produce different hashes |
| **Enforce strong password policies** | Reduces success rate of spraying attacks |
| **Account lockout after N failures** | Slows online brute force; may deter spraying if threshold is low |
| **Multi-factor authentication (MFA)** | A cracked password alone is insufficient — attacker also needs the second factor |
| **Monitor for spraying patterns** | Alert on distributed low-failure-rate login attempts across many accounts |
| **Credential breach monitoring** | Detect if your users' passwords appear in known breach databases |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Plain text storage** | Catastrophic — immediately readable if the database is stolen; stop using any app that does this |
| **Password hashing** | One-way fingerprint — cannot be reversed; different passwords always produce different hashes |
| **Password spraying** | Tries a few common passwords across many accounts — avoids lockout, generates minimal alerts |
| **Brute force** | Tries every possible password — slow online, fast offline with the hash file |
| **Offline brute force** | Attacker cracks hashes locally with no lockout risk — strong hashing algorithms are the primary defense |
| **MFA** | Even a successfully cracked password is useless without the second authentication factor |
