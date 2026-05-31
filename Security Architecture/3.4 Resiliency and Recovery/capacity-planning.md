# Capacity Planning & Scalability

## Overview

**Capacity planning** is the process of ensuring that available resources — people, technology, and infrastructure — match the demand placed on them. Too little capacity causes slowdowns and outages; too much wastes money. The goal is to provision just enough to meet demand, with the flexibility to scale as that demand changes.

---

## The Supply and Demand Balance

| Imbalance | Consequence |
|-----------|-------------|
| **Too little supply** | Application slowdowns, service degradation, outages |
| **Too much supply** | Wasted spend on underutilized resources |
| **Right-sized supply** | Services run smoothly; cost is proportional to actual need |

Achieving the right balance requires accounting for three dimensions: **people**, **technology**, and **infrastructure**.

---

## People

Human resources are one of the most difficult capacity dimensions to scale:

| Direction | Challenge |
|-----------|----------|
| **Scaling up (hiring)** | Recruiting takes time; training takes more time; each new hire is a significant ongoing expense |
| **Scaling down (downsizing)** | People must be redeployed or let go — financial, legal, and organizational costs |
| **Reallocation** | Shifting employees between teams or departments is possible but slow |

Unlike servers, people cannot be added or removed in minutes. Workforce planning must anticipate demand well in advance of when it arrives.

---

## Technology Scalability

Not all technologies scale equally well. Scalability must be a design consideration from the beginning — retrofitting a non-scalable architecture is significantly more expensive than building for scale initially.

### Web / Application Tier

A load balancer with multiple backend servers allows horizontal scaling:

```
[Low demand]
[Load Balancer] ──► [Server A] [Server B]

[High demand]
[Load Balancer] ──► [Server A] [Server B] [Server C] [Server D]
```

Servers are added or removed behind the load balancer without any change visible to users.

### Database Tier

Scaling database capacity options:

| Approach | Description |
|----------|-------------|
| **Multiple database servers** | Distribute read load across replica servers |
| **Database sharding** | Split a database into smaller pieces distributed across multiple servers |
| **Read replicas** | Offload read queries to replicas; writes go to the primary |

### Cloud-Based Services

Cloud infrastructure offers what is effectively unlimited scaling — additional compute, storage, or network resources are available on demand. The constraint is cost, not availability:

- More resources used = more money spent
- Resources can be added or removed in minutes via a management console
- No physical hardware acquisition, shipping, installation, or racking required

---

## On-Premises vs. Cloud Deployment

| Step | On-Premises | Cloud |
|------|------------|-------|
| **Identify need** | Estimate capacity, create purchase order | Estimate capacity |
| **Acquire resources** | Purchase equipment, wait for delivery | Click to provision |
| **Install** | Unbox, rack, cable, power on | Automatic |
| **Configure** | Manual OS and application setup | Template or script-based |
| **Deploy** | Test, then move to production | Test, then deploy |
| **Scale up** | Order more hardware, repeat process | Add instances in minutes |
| **Scale down** | Hardware sits idle (sunk cost) | Remove instances, stop paying |

Cloud's key advantage for capacity planning is **elasticity** — scale up during peak demand, scale down when demand drops, and pay only for what is used.

---

## Rightsizing

**Rightsizing** is the process of continuously matching infrastructure to actual demand — neither over-provisioned nor under-provisioned.

In a physical data center, rightsizing is difficult: hardware is purchased for peak load, then sits underutilized at average load. In the cloud, rightsizing is an ongoing operational process:

```
[Deploy initial capacity for estimated demand]
          |
          ▼
[Monitor actual utilization]
          |
          ├── Demand exceeds capacity → add instances
          |
          └── Demand below capacity → remove instances, reduce cost
```

Cloud providers offer auto-scaling features that can perform this automatically based on defined metrics (CPU utilization, request rate, queue depth, etc.).

---

## Security Considerations in Scaling

When infrastructure scales, security coverage must scale with it:

| Scaling Event | Security Risk If Not Addressed |
|--------------|-------------------------------|
| New server added to load balancer | Server may not have security agent, patches, or monitoring configured |
| New cloud instance provisioned | May be created from an outdated or misconfigured image |
| Database sharded across servers | Security controls must be applied to each shard independently |
| Auto-scaled instances | Automated deployment must include security baseline configuration |

**Best practice:** Use infrastructure as code (IaC) templates that include security configuration — patching, EDR agents, firewall rules, and logging — as part of every instance deployment. This ensures security is never an afterthought when capacity is added.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| **Capacity planning** | Match supply to demand — too little causes outages, too much wastes money |
| **People scaling** | Hardest dimension to scale — hiring and training take time; plan ahead |
| **Horizontal scaling** | Add more servers behind a load balancer — preferred for web and application tiers |
| **Database scaling** | Multiple servers, sharding, and read replicas distribute database load |
| **Cloud elasticity** | Resources added or removed in minutes — pay only for what is used |
| **On-premises scaling** | Hardware must be purchased, shipped, installed, and configured — slow and expensive |
| **Rightsizing** | Continuously match cloud resources to actual demand — auto-scaling automates this |
| **Security at scale** | Security configuration must be part of every deployment template — not applied manually after the fact |
