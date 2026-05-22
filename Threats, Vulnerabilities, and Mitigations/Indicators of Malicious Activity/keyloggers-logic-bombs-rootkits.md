# Keyloggers, Logic Bombs & Rootkits

## Overview

Some of the most dangerous malware operates in silence — capturing data as it's entered, waiting for the right moment to detonate, or hiding so deeply in the operating system that conventional security tools cannot see it. Keyloggers, logic bombs, and rootkits each represent a distinct approach to covert, persistent malicious activity.

---

## Keyloggers

A **keylogger** captures and records every keystroke made on an infected system. Because input typed at the keyboard is never encrypted in transit to the application, keylogging is one of the most effective methods for stealing credentials and sensitive data — bypassing all network encryption entirely.

### What Keyloggers Capture

| Data Type | Examples |
|-----------|---------|
| **Keystrokes** | Usernames, passwords, credit card numbers, messages |
| **Clipboard contents** | Anything copied — passwords, account numbers, private text |
| **Screenshots** | Periodic images of the screen capturing whatever is currently visible |
| **Chat / instant messages** | Conversations in messaging apps |
| **Search queries** | Everything typed into search engines |
| **Visited URLs** | Web browsing history |

### How Keyloggers Operate

Keylogging software runs persistently in the background, recording all captured data to a local file. That file is then transmitted to the attacker — typically once or more per day — providing a complete record of everything the user has typed.

**Example:** DarkComet is a well-known Remote Access Trojan (RAT) that includes keylogging functionality. It captures keystrokes with timestamps and even records editing actions (spaces, deletions) so the attacker sees exactly what was typed, corrected, and finalized.

### Why Keyloggers Are Effective

Encryption protects data in transit and at rest — but **at the keyboard, nothing is encrypted**. A user who carefully uses HTTPS, a VPN, and full-disk encryption still types their password in plaintext before any of those protections apply. That is the window keyloggers exploit.

---

## Logic Bombs

A **logic bomb** is malicious code that lies dormant until a specific trigger condition is met. When the condition is satisfied, the bomb executes its payload — which may include deleting files, wiping drives, corrupting data, or taking down systems.

### Common Trigger Types

| Trigger | Example |
|---------|---------|
| **Date and time** | Activates at a specific date and time |
| **User login** | Executes when a particular account logs in |
| **File access** | Triggers when a specific file is opened or deleted |
| **System event** | Fires on shutdown, startup, or a specific process |

### Why Logic Bombs Are Hard to Detect

Logic bombs are typically written by insiders or targeted attackers with specific goals — not distributed malware. Because they are custom-built for a particular environment, **no antivirus signature exists** for them. Traditional anti-malware software cannot identify code it has never seen before.

### Detection Approaches

- **File integrity monitoring** — track changes to critical OS and application files; unexpected modifications may indicate a planted bomb
- **Least privilege enforcement** — limit which users can modify system files, reducing the opportunity to plant a bomb
- **Change control processes** — require approval and logging for any changes to core system components
- **User rights auditing** — regularly review permissions to ensure no user has more access than needed

### Real-World Example: South Korea Bank & Broadcasting Attack (2013)

| Event | Detail |
|-------|--------|
| **March 19, 2013** | Malicious email sent to banks and broadcasting companies with an infected attachment |
| **Infection** | Opening the attachment installed a Trojan on the user's system |
| **March 20, 2013 — 2:00 PM local time** | Logic bomb activated — deleted all storage data and the master boot record |
| **Effect** | Systems rebooted to no operating system; ATMs across the bank displayed "Boot device not found" |

The logic bomb was precisely timed, activated simultaneously across multiple systems, and rendered both bank workstations and ATMs completely inoperable.

---

## Rootkits

A **rootkit** is malware that embeds itself at the deepest level of the operating system — typically the **kernel** — to hide its presence and maintain persistent, privileged access. The name comes from "root," the Unix superuser account equivalent to Administrator in Windows.

### Why Rootkits Are So Dangerous

| Property | Impact |
|----------|--------|
| **Kernel-level integration** | Runs as part of the OS itself — not as a separate process |
| **Invisible to process lists** | Does not appear in task managers or process monitors |
| **Invisible to antivirus** | Traditional security tools query the OS for process information — which the rootkit controls |
| **Full system access** | Operates with the same privileges as the OS kernel |

Because the rootkit *is* part of the operating system from the OS's perspective, asking the OS to help detect it is fundamentally unreliable — the rootkit controls what the OS reports.

### Detection & Removal

- **Rootkit-specific removal tools** — targeted tools exist for known rootkit variants; useful for remediation after infection
- **Offline scanning** — scanning a drive from a separate, clean OS (e.g., bootable media) bypasses the compromised OS entirely
- **Reinstallation** — in severe cases, a clean OS installation is the only reliable path to a trusted system state

### Secure Boot

**Secure Boot** (implemented in the UEFI BIOS) is a preventive control specifically designed to counter rootkits that target the boot process:

```
[System powers on]
          |
          v
[UEFI checks OS signature against known-good cryptographic hash]
          |
          +-- Signature valid --> OS boots normally
          |
          +-- Signature invalid / modified --> Boot halted
                    |
                    v
              [Rootkit prevented from loading]
```

Even if a rootkit successfully installs itself into the kernel, Secure Boot will detect the modification at startup and prevent the system from booting with the compromised OS — stopping the rootkit before it can execute.

---

## Comparison

| Property | Keylogger | Logic Bomb | Rootkit |
|----------|-----------|-----------|---------|
| **Primary purpose** | Data capture and exfiltration | Timed destructive payload | Persistent hidden access |
| **Detection difficulty** | Moderate | High — no signatures | Very high — hides from OS |
| **Trigger** | Continuous (always running) | Conditional (event-based) | Persistent (always active) |
| **Typical origin** | Distributed malware / RATs | Targeted / insider threat | Sophisticated attackers |
| **Key defense** | Anti-malware, user training | File integrity monitoring, least privilege | Secure Boot, offline scanning |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Keylogger** | Records all keystrokes and other input; bypasses encryption by capturing data before it is protected |
| **DarkComet** | A well-known RAT with integrated keylogging capabilities |
| **Logic bomb** | Dormant malicious code that activates on a trigger; no antivirus signature — detected through monitoring |
| **South Korea (2013)** | Logic bomb wiped bank systems and ATMs simultaneously at a precise time |
| **Rootkit** | Embeds in the OS kernel; invisible to conventional tools; operates with full OS-level privileges |
| **Secure Boot** | UEFI feature that validates OS integrity at startup — prevents modified kernels from loading |
