# Viruses & Worms

## Overview

Viruses and worms are both self-replicating forms of malware, but they differ in one critical way: **viruses require human interaction to spread, worms do not**. Understanding the distinction matters for both detection and defense.

---

## Viruses

A **computer virus** replicates itself by attaching to files or programs and spreading when those files are executed or shared. Like a biological virus, it needs a host — and in most cases, it needs a human to activate it.

### How Viruses Spread

Most virus infections begin with a user action:
- Clicking a malicious link in an email or on a website
- Running an infected executable
- Opening a document that contains a malicious macro

Once active, the virus can traverse the local file system and attempt to spread to other systems via network shares, removable drives, or email.

### Types of Viruses

| Type | How It Works |
|------|-------------|
| **Program / executable virus** | Attaches to or replaces a legitimate executable; runs when the user launches the program |
| **Boot sector virus** | Infects the boot sector of a storage drive; runs automatically when the system starts — before the OS loads |
| **Script virus** | Written in a scripting language (JavaScript, VBScript, etc.); executes within a browser, OS, or application |
| **Macro virus** | Written in a macro language (e.g., Microsoft Office VBA); executes when an infected document is opened |
| **Fileless virus** | Never writes to disk — operates entirely in memory to evade file-based antivirus detection |

---

## Fileless Viruses

A **fileless virus** is a particularly stealthy variant that avoids writing any malicious code to disk. Because most antivirus software monitors files being written to storage, a virus that lives entirely in memory can bypass those checks.

### How a Fileless Virus Operates

```
[User clicks malicious link in email or on a website]
          |
          ▼
[Browser loads malicious site — exploits vulnerability in Flash, Java, or OS]
          |
          ▼
[Malicious code runs in memory — nothing written to disk]
          |
          ▼
[Code launches legitimate system tools (e.g., PowerShell)]
          |
          ▼
[PowerShell downloads and executes additional scripts — in memory]
          |
          ▼
[Virus installs further payloads, exfiltrates data, establishes persistence]
          |
          ▼
[Persistence: registry autostart entry added so virus reloads on reboot]
```

Because the malicious code itself is never stored as a file, it is significantly harder to detect with traditional signature-based antivirus tools. The registry autostart entry is typically the only artifact left on disk.

---

## Antivirus & Signatures

Antivirus software continuously monitors running processes and files for patterns that match known malicious code — these patterns are called **signatures**. This is why keeping signature files up to date is critical:

- New malware is released constantly
- Outdated signatures cannot detect new threats
- Many modern antivirus solutions also use behavioral detection to catch unknown malware, including fileless variants

---

## Worms

A **worm** is malware that can **self-replicate and spread across systems without any user interaction**. It exploits vulnerabilities in network-connected software to copy itself from machine to machine automatically.

### Key Characteristics

| Property | Description |
|----------|-------------|
| **No user action required** | Spreads entirely on its own using software vulnerabilities |
| **Network-driven** | Uses the network to propagate — infects every reachable vulnerable system |
| **Speed** | Replicates at network speed; infections can spread across an entire network in minutes |
| **Any time** | Operates without waiting for a user to do anything — active 24/7 |

### Defenses Against Worms

- **Network firewalls** — restrict lateral traffic between systems, limiting propagation
- **Personal firewalls** — block inbound connections on individual hosts
- **Intrusion Prevention Systems (IPS)** — detect and block worm traffic patterns
- **Patching** — worms exploit known vulnerabilities; patched systems are not vulnerable

---

## Real-World Example: WannaCry

**WannaCry** is one of the most damaging worms ever documented, combining self-propagation with ransomware to devastating effect.

### How WannaCry Worked

```
[Infected system scans network for vulnerable machines]
          |
          ▼
[Exploits EternalBlue — a vulnerability in Windows SMB protocol]
          |
          ▼
[EternalBlue installs a backdoor on the target system]
          |
          ▼
[Backdoor downloads and installs ransomware]
          |
          ▼
[Ransomware encrypts all user files on the newly infected system]
          |
          ▼
[Newly infected system begins scanning for more vulnerable machines]
          |
          ▼
[Cycle repeats across the entire network]
```

WannaCry combined two attack types:
- **Worm behavior** — self-propagating via EternalBlue with no user interaction
- **Ransomware** — encrypted all user files and demanded payment for recovery

The result was rapid, automated infection across entire networks, with data simultaneously held for ransom on every compromised machine.

---

## Virus vs. Worm: Quick Comparison

| Property | Virus | Worm |
|----------|-------|------|
| **Requires user action** | Yes | No |
| **Self-replicates** | Yes (via user interaction) | Yes (automatically) |
| **Spread speed** | Slower — depends on user behavior | Fast — network speed |
| **Primary defense** | Antivirus, user training | Firewalls, IPS, patching |
| **Fileless variant** | Yes | Less common |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Virus** | Requires user interaction to activate and spread; many types including boot sector, macro, script, and fileless |
| **Fileless virus** | Operates entirely in memory — evades file-based antivirus; uses system tools like PowerShell |
| **Antivirus signatures** | Must be kept current to detect new threats; behavioral detection helps catch unknown variants |
| **Worm** | Self-propagating malware requiring no user interaction — spreads at network speed |
| **WannaCry** | Combined EternalBlue worm propagation with ransomware — rapidly encrypted files across entire networks |
| **Worm defense** | Network segmentation, firewalls, IPS, and prompt patching of known vulnerabilities |
