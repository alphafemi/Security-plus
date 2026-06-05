# Active Directory, Group Policy & SELinux

## Overview

Managing identity, authentication, and access control across hundreds or thousands of devices requires centralized systems. **Active Directory** provides a central identity store for Windows environments. **Group Policy** overlays security and configuration settings on users and computers managed by Active Directory. **SELinux** provides mandatory access control for Linux systems, enforcing least privilege at the OS level.

---

## Active Directory

**Active Directory (AD)** is a centralized database that stores information about every component of a Windows network — and serves as the foundation for authentication and authorization.

### What Active Directory Stores

| Object Type | Description |
|-------------|-------------|
| **User accounts** | Usernames, passwords, account settings for every user |
| **Computer accounts** | Every Windows device joined to the domain |
| **Groups** | Collections of users or computers for permission assignment |
| **File shares and printers** | Network resources available to domain members |
| **Security policies** | Baseline security configurations applied to users and devices |
| **Organizational Units (OUs)** | Logical containers for organizing objects in the directory |

### What Active Directory Enables

| Function | Description |
|----------|-------------|
| **Centralized authentication** | Users log in once with domain credentials — same username/password works across all domain resources |
| **Permission assignment** | Assign access rights to individual users or to groups |
| **Account management** | Create, modify, and disable accounts from one central console |
| **Group-based access control** | Add users to groups; groups receive permissions — simplifies management at scale |

### How Centralized Authentication Works

```
[User logs in to workstation]
          |
          ▼
[Workstation sends credentials to Active Directory Domain Controller]
          |
          ▼
[AD validates username + password against directory database]
          |
          ├── Valid → Kerberos ticket issued; access granted
          |
          └── Invalid → Authentication denied
```

Once authenticated, the user's group memberships determine what resources they can access — across all domain-joined systems.

---

## Group Policy

**Group Policy** is a mechanism for centrally managing configuration settings and security policies for users and computers in an Active Directory domain.

### What Group Policy Controls

| Category | Examples |
|----------|---------|
| **Login scripts** | Scripts that run automatically when a user logs in |
| **Network configuration** | Drive mappings, printer connections, quality of service |
| **Security settings** | Password policies, account lockout, audit logging |
| **Software deployment** | Applications automatically installed on managed computers |
| **Desktop restrictions** | Prevent access to Control Panel, restrict installed applications |
| **Windows Firewall settings** | Enforce firewall rules across all managed devices |

### How Group Policy Is Applied

```
[Administrator creates/modifies Group Policy Object (GPO) in Group Policy Management Editor]
          |
          ▼
[GPO linked to an Organizational Unit (OU), domain, or site]
          |
          ▼
[Policy applies to all users and computers in that OU]
          |
          ▼
[Devices and users receive updated policies at next login or policy refresh]
```

### Group Policy Hierarchy

Policies can be applied at multiple levels and are inherited from parent containers:

| Level | Scope |
|-------|-------|
| **Local** | Applies to a single computer |
| **Site** | Applies to a physical AD site |
| **Domain** | Applies to all objects in the domain |
| **Organizational Unit (OU)** | Applies to specific groups of users or computers |

More specific (OU-level) policies override less specific (domain-level) policies where they conflict.

---

## Active Directory + Group Policy: Combined Power

Together, AD and Group Policy create a comprehensive control mechanism:

| Task | How AD + GP Handles It |
|------|----------------------|
| Enforce password complexity across all users | Domain-level Group Policy → Password Policy |
| Give the finance team access to finance shares | Create a "Finance" security group in AD; assign permissions to that group |
| Require screen lock after 5 minutes of inactivity | Group Policy → Screen saver/lock settings |
| Automatically install antivirus on all new workstations | Group Policy → Software installation |
| Restrict users from installing their own software | Group Policy → Software restriction policies |

---

## SELinux — Security-Enhanced Linux

By default, Linux uses **Discretionary Access Control (DAC)** — users can set permissions on their own files and resources as they choose. In high-security environments, this flexibility creates risk.

**SELinux** replaces DAC with **Mandatory Access Control (MAC)** — where a central administrator defines all access rules, and users cannot override them.

### DAC vs. MAC

| Property | Discretionary Access Control (DAC) | Mandatory Access Control (MAC) |
|----------|-----------------------------------|-------------------------------|
| **Who sets permissions** | The resource owner (the user) | Central administrator |
| **User override** | Yes — users can change their own file permissions | No — users cannot override centrally set policies |
| **Typical environment** | Standard Linux, standard Windows | High-security environments, government, classified systems |
| **Example** | User can `chmod 777` a file to make it world-readable | Policy prevents that regardless of user action |

### How SELinux Enforces Least Privilege

SELinux assigns a **security context** (label) to every process, file, and resource. Access is only permitted when the policy explicitly allows it — everything else is denied.

```
[Process requests access to a resource]
          |
          ▼
[SELinux checks policy: does this process's label have permission
 to access this resource's label?]
          |
          ├── Policy permits → access granted
          |
          └── Policy denies → access blocked (even if DAC would allow it)
```

### Benefits of SELinux

| Benefit | Description |
|---------|-------------|
| **Least privilege** | Processes and users can only access what the policy explicitly permits |
| **Breach containment** | If a process is compromised, the attacker is limited to what that process's policy allows |
| **Malware limitation** | Malicious code running under a restricted context cannot access other parts of the system |

### SELinux Availability

SELinux is:
- **Open source** — freely available
- **Widely distributed** — included or available for many major Linux distributions (RHEL, CentOS, Fedora, Ubuntu)
- **Configurable** — can be set to enforcing (blocks violations) or permissive (logs violations without blocking) mode

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Active Directory** | Central database of all users, computers, and resources in a Windows domain |
| **Centralized authentication** | Single username/password works across all domain resources via Kerberos |
| **Group Policy** | Overlays security settings and configurations on users and devices managed by AD |
| **GPO hierarchy** | Local → Site → Domain → OU; more specific settings override less specific |
| **DAC (Linux default)** | Users control permissions on their own resources — flexible but risky |
| **MAC (SELinux)** | Administrator controls all permissions — users cannot override |
| **SELinux** | Open-source MAC implementation for Linux; enforces least privilege at the OS level |
| **Breach containment** | SELinux limits damage from compromised processes to only what their security context permits |
