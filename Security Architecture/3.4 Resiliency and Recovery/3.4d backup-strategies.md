# Backup Strategies & Data Recovery

## Overview

Backups are the last line of defense when all other security controls fail. A backup that was never taken, never tested, or can't be restored is worthless when a disaster strikes. Effective backup strategy requires planning across multiple dimensions: location, frequency, type, encryption, and validation.

---

## Backup Planning Dimensions

Before implementing any backup solution, answer these questions:

| Dimension | Questions to Answer |
|-----------|-------------------|
| **Volume** | How much data needs to be backed up? Megabytes, terabytes, more? |
| **Type** | Full backup? Incremental? Differential? Snapshot? |
| **Media** | Local disk, tape, cloud? |
| **Location** | On-site, off-site, or both? |
| **Software** | Built-in OS tools or third-party? What restores with what? |
| **Schedule** | How often? Different schedules for different systems? |
| **Security** | Is backup data encrypted? Who has access? |

---

## Backup Locations

### On-Site Backup

Data and backup media stored at the same location as the primary system.

**Advantages:**
- Fast restoration — no network transfer required
- Lower cost — no third-party storage fees
- Immediately accessible

**Limitation:** Vulnerable to the same physical disasters (fire, flood, theft) as primary systems.

### Off-Site Backup

Data transferred to a geographically separate location — either via network or physical media transport.

**Advantages:**
- Protected from local disasters
- Accessible from anywhere via network connection
- Can serve as a disaster recovery foundation

**Limitation:** Slower restoration due to network transfer; physical media transport has its own risks (see below).

### Both (Recommended)

| Use | Strategy |
|-----|---------|
| **On-site** | Fast restore for day-to-day recovery (accidental deletion, hardware failure) |
| **Off-site** | Long-term retention; protection against site-level disasters |

---

## Backup Frequency & Retention

Frequency depends on how much data change is acceptable to lose if a system must be restored.

| Frequency | Use Case |
|-----------|---------|
| **Hourly** | High-change systems where losing hours of data is unacceptable |
| **Daily** | Standard for most production systems |
| **Weekly** | Stable systems with infrequent changes |
| **Monthly** | Long-term retention or archival purposes |

### Multiple Backup Sets

Organizations commonly maintain multiple overlapping retention sets:

- 30 daily backups (one per day for the last month)
- 4 weekly backups (one per week for the last month)
- 12 monthly backups (one per month for the last year)

This provides granular recovery options for recent events and longer-term archival for older data.

---

## Backup Encryption

Backup media contains some of the most sensitive data in the organization. If it falls into the wrong hands, the consequences are identical to a live data breach.

**Real-world risk:** Backup tapes have been stolen from vehicles while being transported to off-site storage. If unencrypted, the data is immediately accessible to the thief.

| Storage Location | Encryption Requirement |
|-----------------|----------------------|
| **On-site** | Strongly recommended |
| **Off-site (physical transport)** | Critical — physical media can be stolen in transit |
| **Cloud storage** | Essentially required — the cloud provider's employees and systems have access to stored data |

> **Critical:** Encryption keys must be stored separately from the backup data — and must be accessible when you need to restore. An encrypted backup with a lost key is unrestorable.

---

## Backup Types

### Full Backup

Copies all data every time. Slowest and most storage-intensive, but restoration is straightforward — use the most recent full backup.

### Incremental Backup

Copies only data that changed since the **last backup** (full or incremental). Fast to create, storage-efficient, but restoration requires the last full backup plus every incremental since then.

### Differential Backup

Copies only data that changed since the **last full backup**. Takes longer than incremental but restoration only requires the last full backup plus the most recent differential.

### Snapshot

A point-in-time capture of a virtual machine or storage volume — common in virtualized and cloud environments.

- Created with a single operation — effectively one-click backup
- Stores only what has changed since the previous snapshot (incremental storage)
- Easy to apply — rolling back to a previous snapshot takes seconds
- Useful before any significant system change

**Daily snapshot example:**

| Day | Snapshot Size | Contents |
|-----|-------------|----------|
| Monday (initial) | 100 GB | All data on the VM |
| Tuesday | 40 GB | Only the 40 GB that changed since Monday |
| Wednesday | 20 GB | Only the 20 GB of new data added Wednesday |

Each snapshot is a complete restore point, but only stores the delta since the previous one.

---

## Replication

**Replication** copies data to one or more remote locations in near real time. Unlike periodic backups, replication keeps the remote copy continuously up to date.

```
[Primary Site — data changes]
          |
          | (changes replicated in near real time)
          ▼
[Replica Site — always current]
[Replica Site 2 — always current]
```

### Use Cases

| Use | Description |
|-----|-------------|
| **Hot site** | Replicated data means the hot site is always ready for immediate failover |
| **Disaster recovery** | If the primary site fails, the replica has current data |
| **Geographic redundancy** | Data exists in multiple locations simultaneously |

**Limitation:** If data is corrupted or deleted at the primary site, that corruption replicates to the remote copy. Replication is not a substitute for periodic backups — it complements them.

---

## Journaling

**Journaling** protects against data corruption from power failures or crashes during write operations.

### The Problem Without Journaling

If power fails while writing data to a database, some data is written and some is not — leaving the database in a corrupted, partially-updated state.

### How Journaling Works

```
[Application writes data]
          |
          ▼
[Data written to journal first] ← if power fails here, journal is lost
          |                       but database is unchanged (no corruption)
          ▼
[Journal data written to database] ← if power fails here,
          |                          database has partial write
          ▼
[On restart: system checks journal]
  - Finds incomplete write
  - Uses journal to complete the database update
  - Database is corrected automatically
          |
          ▼
[Journal cleared — process repeats]
```

Journaling ensures that after any failure, the database can always be restored to a consistent state without manual intervention.

---

## Backup Testing

A backup that has never been tested may not actually be restorable. Testing is not optional:

| Test Type | What It Verifies |
|-----------|----------------|
| **Restore test** | Data can actually be read back from the backup |
| **Application validation** | Restored data works correctly with the application |
| **Full system recovery** | An entire system can be rebuilt from backup |
| **Periodic schedule test** | All backup sets (daily, weekly, monthly) are tested regularly |

> Discovering that backups are corrupted or unrestorable during an actual disaster is too late. Test restores before you need them.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **On-site backup** | Fast restoration; vulnerable to local disasters |
| **Off-site backup** | Protected from local disasters; slower restoration |
| **Backup encryption** | Essential — backup media contains full copies of sensitive data |
| **Snapshot** | Point-in-time VM/volume capture; incremental storage; one-click rollback |
| **Replication** | Near-real-time copy to remote site; complements but does not replace periodic backups |
| **Journaling** | Protects against database corruption from power failures during write operations |
| **Backup testing** | Backups must be tested regularly — an untested backup may be unrestorable |
| **Multiple retention sets** | Daily, weekly, and monthly sets provide granular and long-term recovery options |
