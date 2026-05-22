# Indicators of Compromise (IOC)

## Overview

An **Indicator of Compromise (IOC)** is evidence that a system, network, or account has likely been breached. IOCs are not certainties — some may have innocent explanations — but each one warrants investigation. Identifying and acting on IOCs quickly limits the damage an attacker can do once inside.

Security teams monitor for IOCs continuously, using logs, alerts, and anomaly detection to catch signs of intrusion as early as possible.

---

## Common Indicators of Compromise

### 1. Account Lockouts

An account locking out due to repeated failed login attempts — when the legitimate user was not the one making those attempts — is a strong IOC.

| Scenario | Likely Cause |
|----------|-------------|
| Account locked after failed attempts | Brute force or credential stuffing attack in progress |
| Account administratively disabled without authorization | Attacker may have used system access to disable the account |
| Lockout followed by help desk call to reset password | Social engineering — attacker locks the account, then impersonates the user to get a reset |

> **Best practice:** Enforce strict identity verification procedures for all password resets. A lockout followed by a reset request is a classic social engineering setup.

---

### 2. Concurrent Sessions from Impossible Locations

A single user account showing active sessions from two geographically distant locations within an impossibly short time frame is a reliable IOC.

**Example:** A user logs in from Omaha, Nebraska, then three minutes later a login appears from Australia. No person can travel that distance in three minutes — one of those sessions is not the legitimate user.

This is sometimes called an **impossible travel** alert and can be detected by comparing authentication log entries (timestamp + IP geolocation) for the same account.

> Note: Users with multiple devices (desktop, laptop, mobile) may show simultaneous logins from different locations. Context matters — impossible travel across continents in minutes is the red flag.

---

### 3. Blocked Security Updates

Malware commonly disables security updates and antivirus signature downloads immediately after infecting a system. The attacker's goal is to prevent the vulnerability they exploited from being patched and to prevent the malware from being detected by updated signatures.

**IOC signals:**
- Inability to connect to antivirus update servers
- Security patches failing to download or install
- Antivirus software reporting it cannot update

If users or endpoints cannot reach security-related websites or update services, treat it as suspicious until proven otherwise.

---

### 4. Unusual Resource Consumption

Attackers in a network are always doing something — copying files, transferring data, scanning systems. These activities consume resources and leave traces.

| Activity | Observable IOC |
|----------|---------------|
| **Data exfiltration** | Sudden spike in outbound network traffic, especially at unusual hours |
| **File transfers between systems** | Elevated internal network traffic between systems that don't normally communicate |
| **Scanning or probing** | Unusual connection attempts across many ports or hosts |
| **3:00 AM traffic spike** | Significant network activity during times when no legitimate work should be occurring |

> A single small file transfer at 3:00 AM can be the first — and sometimes only — sign that data is leaving your network. Baseline normal traffic patterns to make anomalies detectable.

---

### 5. Unavailable or Crashed Resources

A server or service that suddenly becomes unreachable may indicate attacker activity:

| Cause | What It Suggests |
|-------|-----------------|
| **Server crash during probing** | Attacker triggered a vulnerability while scanning — the crash itself is the IOC |
| **Network segment disruption** | Attacker creating network noise as a distraction while exploiting elsewhere |
| **Ransomware** | Files encrypted and inaccessible; entire systems may go offline |
| **Account lockout** | Server inaccessible because brute force attempts locked the service account |

---

### 6. Out-of-Cycle Log Entries

Logs record what happens and when. If log entries appear at times that don't align with expected operational patterns, it may indicate unauthorized activity.

**Examples:**
- Security patches appearing in logs outside the scheduled maintenance window
- Application installations logged at 2:00 AM when no deployments were authorized
- Firewall logs showing traffic flows during periods of zero expected activity

Organizations with a **change control process** can immediately identify log entries that fall outside approved change windows — these are inherently suspicious.

---

### 7. Missing Log Data

Attackers know that their activity generates log entries. A common cleanup step after a breach is **deleting logs** to erase evidence.

```
[Attacker authenticates] → log entry created
[Attacker transfers files] → log entry created
[Attacker accesses server] → log entry created
[Attacker deletes logs] → log entries gone
```

**IOC:** Gaps in log continuity — periods where logging should have occurred but no entries exist.

**Best practices:**
- Send logs to a **centralized, write-protected log server** that local users (and malware) cannot modify
- Set up **alerts for missing log data** — a gap in logging is itself an alert
- Compare log volumes across time — a sudden drop in log entries is suspicious

---

### 8. Data Appearing Publicly

One of the most definitive IOCs is discovering that private organizational data has been published on the internet. By the time this is discovered, the breach has likely been ongoing for some time.

**Double extortion ransomware:** Attackers increasingly combine data exfiltration with ransomware:

```
[Attacker gains access to network]
          |
          ▼
[Exfiltrates all sensitive data to attacker-controlled servers]
          |
          ▼
[Deploys ransomware — encrypts all files]
          |
          ▼
[Demands payment for decryption key]
          |
          ▼
[Threatens to release stolen data publicly if not paid]
```

Even if the victim restores from backup and avoids paying for decryption, the threat of public data release provides additional leverage.

---

## IOC Summary

| Indicator | What to Look For |
|-----------|-----------------|
| **Account lockout** | Lockouts the legitimate user didn't cause; unusual admin disabling of accounts |
| **Impossible concurrent logins** | Same account active in two distant locations within an implausibly short time |
| **Blocked security updates** | Endpoints unable to reach update or antivirus servers |
| **Unusual resource consumption** | Traffic spikes, especially outbound and during off-hours |
| **Unavailable resources** | Unexpected server crashes, inaccessible files, locked accounts |
| **Out-of-cycle log entries** | Activity in logs that falls outside expected operational patterns |
| **Missing log data** | Gaps in log continuity — evidence of log deletion |
| **Public data exposure** | Organizational data appearing on public internet or dark web |

---

## Detection Best Practices

| Practice | Benefit |
|----------|---------|
| **Centralized, protected logging** | Prevents attackers from deleting logs on individual systems |
| **Alert on log gaps** | Detects log deletion attempts |
| **Baseline normal traffic patterns** | Makes anomalous spikes detectable |
| **Authentication log review** | Enables impossible travel and concurrent session detection |
| **Change control process** | Makes out-of-cycle activity immediately apparent |
| **Data loss prevention (DLP) tools** | Detects and blocks large-scale exfiltration attempts |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **IOC** | Evidence that a system or account has likely been compromised — warrants immediate investigation |
| **Impossible travel** | Same account active in two distant locations in an impossibly short time — strong sign of credential theft |
| **Blocked updates** | Malware disables security updates to preserve its foothold |
| **Off-hours traffic** | Data exfiltration often occurs at night when it's less likely to be noticed |
| **Log deletion** | Attackers routinely delete logs — gaps in logging are themselves an IOC |
| **Double extortion** | Ransomware attackers exfiltrate data before encrypting, then threaten public release for additional leverage |
