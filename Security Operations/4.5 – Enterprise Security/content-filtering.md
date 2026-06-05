# Content Filtering & URL Filtering

## Overview

**Content filtering** controls what information users can access through their browsers and applications. It goes beyond firewall port-based rules to evaluate the actual content and destination of web traffic — blocking malicious sites, inappropriate content, or any category an organization deems off-limits.

---

## Types of Content Filtering

| Filter Type | How It Works | Primary Use |
|-------------|-------------|------------|
| **URL / website filter** | Blocks or allows access to specific URLs or site categories | Controlling web browsing |
| **Content filter** | Evaluates data going in or out of the network | Protecting sensitive data; parental controls |
| **DNS filter** | Prevents DNS resolution for blocked domains | Blocking malicious sites and C2 servers |
| **Proxy-based filter** | Intercepts all web traffic; scans responses | URL filtering + malware scanning + caching |

---

## URL Filtering

**URL filtering** allows or blocks web access based on the destination URL or a category the URL belongs to.

### Allow Lists and Block Lists

- **Allow list:** Only specified URLs are accessible — everything else is blocked
- **Block list:** Specified URLs are blocked — everything else is accessible

Managing individual fully qualified domain names (FQDNs) at scale is impractical. Most URL filter products use **category-based filtering** instead.

### Category-Based Filtering

Common URL categories (most products have 50+):

| Example Category | Typical Policy |
|-----------------|---------------|
| Educational | Allow |
| Government | Allow |
| Home and garden | Allow (log access) |
| Travel | Allow |
| Auction | Allow or log |
| Gambling | Block |
| Hacking | Block |
| Malware / phishing | Block |
| Adult content | Block |

Category-based filtering lets administrators set broad policies without managing thousands of individual URLs.

---

## Reputation-Based Filtering

Beyond categories, content filters can evaluate a site's **reputation** — a risk score based on automated analysis of the site's content and behavior.

| Reputation Level | Typical Action |
|-----------------|---------------|
| **Trustworthy** | Allow |
| **Low risk** | Allow |
| **Medium risk** | Allow with logging |
| **Suspicious** | Alert / review |
| **High risk** | Block |

Reputation scores are assigned automatically through scanning — with millions of sites, manual review is not feasible. Administrators can manually override automated scores for specific URLs.

---

## Where Filtering Is Applied

### Firewall-Integrated URL Filtering (NGFW)

Modern NGFWs include URL filtering built in — one device handles firewall rules, IPS, and URL filtering together.

**Limitation:** Only filters traffic passing through the firewall. Remote or mobile users on other networks bypass the filter.

### Agent-Based Content Filtering

Software agent installed on each user's device — filtering follows the user regardless of network.

```
[User travels / works from home]
      |
[Agent on user's device checks URL]
  ↓ blocked? → user sees block page
  ↓ allowed? → traffic proceeds
```

- Works on any network — not limited to corporate firewall
- Requires regular updates to category databases on each agent
- Managed from a central console

### Proxy-Based Filtering

A **forward proxy** sits between users and the internet, intercepting all web requests.

```
[Internal User]
      |
      | (request for website)
      ▼
[Forward Proxy]
  - URL / category check
  - Reputation check
  - Malware scan on response content
  - Caching (serve cached content for repeat requests)
      |
      ▼
[Internet website]
      |
      ▼ (response returns to proxy)
[Proxy evaluates response → sends to user if clean]
```

**Proxy advantages over firewall-only filtering:**
- Can inspect the actual content of responses (not just the destination)
- Caches frequently requested content — reduces bandwidth
- Provides access control by username/password or IP
- Can scan for malware in downloaded files

### Explicit vs. Transparent Proxy

| Type | Configuration | User Awareness |
|------|-------------|----------------|
| **Explicit** | Client must specify proxy address in browser/application settings | User knows proxy exists |
| **Transparent** | No client configuration — proxy intercepts automatically | User has no idea |

---

## DNS Filtering

**DNS filtering** works at the Domain Name System level — when a user tries to visit a blocked domain, the DNS server simply doesn't return an IP address.

### How DNS Filtering Works

```
[User's browser requests www.malicioussite.org]
          |
          ▼ (DNS query)
[DNS Filter Server]
  - checks domain against block list
  - updates continuously via real-time threat intelligence
          |
          ├── Domain is blocked → no IP returned (or returns a block page IP)
          │         User cannot reach the site
          |
          └── Domain is allowed → returns correct IP
                    User reaches the site normally
```

### DNS Filter Advantages

| Advantage | Detail |
|-----------|--------|
| **Works for all protocols** | Not just web traffic — any DNS lookup is filtered |
| **Blocks malware C2 communication** | Malware that tries to call home via DNS is stopped at the lookup |
| **Real-time threat intelligence** | Block lists updated continuously; new malicious domains blocked quickly |
| **No client configuration needed** | Works by pointing devices to the filtering DNS server |
| **Lightweight** | No proxy or agent required |

### DNS Filtering vs. URL Filtering

| Property | DNS Filtering | URL Filtering |
|----------|--------------|--------------|
| **Works at** | Domain level | Full URL level |
| **Can filter by path** | No (e.g., can't allow example.com/safe but block example.com/bad) | Yes |
| **Covers all protocols** | Yes | Typically HTTP/HTTPS only |
| **Blocks malware C2** | Yes — any DNS-using malware | Partial |
| **Requires client agent** | No | Sometimes |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **URL filtering** | Allow/block by specific URL or category; most products have 50+ categories |
| **Category-based filtering** | Group thousands of URLs into manageable categories for policy decisions |
| **Reputation filtering** | Block sites scored as suspicious or high-risk; automatically updated |
| **NGFW-integrated** | One device handles firewall + IPS + URL filtering; only covers traffic through the firewall |
| **Agent-based** | Follows the user to any network; requires regular updates to category databases |
| **Forward proxy** | Intercepts all web traffic; adds content scanning, caching, and access control |
| **DNS filtering** | Prevents DNS resolution for blocked domains; covers all DNS-using protocols including malware C2 |
