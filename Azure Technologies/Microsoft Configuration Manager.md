---
tags:
  - sc100
type: service
domain:
  - infrastructure
aliases:
  - Configuration Manager
  - ConfigMgr
  - SCCM
  - MECM
  - MCM
  - System Center Configuration Manager
---

# Microsoft Configuration Manager

## Purpose

An on-prem systems management suite (part of the **Microsoft Intune family**) that deploys apps, **software updates** and operating systems, and manages compliance for Windows servers, desktops and laptops.

---

## Why Architects Choose It

- Deep existing investment in **on-prem Windows management**: OS deployment, app packaging, compliance settings, software updates.
- **Content distribution at scale**: distribution points and peer caching (BranchCache, Delivery Optimization) for low-bandwidth or disconnected sites.
- A migration path to cloud via **co-management** with [[Intune]], sliding workloads gradually.
- Granular update control: **Automatic Deployment Rules (ADRs)**, **phased deployments**, **orchestration groups**.

---

## Key Components

| Component | Role |
| --- | --- |
| Site hierarchy | Central Administration Site → Primary → Secondary sites. The top site syncs updates first. |
| **Software Update Point (SUP)** | Site role built on **[[WSUS]]**. Clients scan WSUS for updates. |
| Distribution Points | Host content close to clients |
| SQL Server + SSRS | Site database and reporting |
| Software Center | End-user self-service portal (apps, updates, OS) |
| Integrations | [[Intune]] (co-management), WSUS, AD DS + [[Entra ID]] (discovery), Group Policy, WDS/ADK, certificate services |

---

## When to Use

- Large on-prem Windows fleets with existing ConfigMgr.
- Disconnected or bandwidth-constrained sites that need local content.
- OS deployment/imaging and complex app packaging.
- Phased/orchestrated patching sequences (e.g. cluster nodes in order).

## When NOT to Use

- **Linux patching**: ConfigMgr software updates don't support Linux → [[Azure Update Manager]].
- Greenfield cloud-first: [[Intune]] for clients, [[Azure Update Manager]] for servers.
- Alongside AUM **on the same servers for updates**. Choose one patching authority.
- Internet-only/BYOD mobile management → Intune.

---

## Architecture Decisions

```
Existing ConfigMgr + want cloud?
  ↓
Co-management → slide workloads to Intune

Servers managed by ConfigMgr, want Azure-native patching?
  ↓
Azure Update Manager (+ Arc); ConfigMgr keeps non-update roles

Azure equivalents of ConfigMgr capabilities:
  Update Manager + Change Tracking & Inventory +
  Machine Configuration + Azure Automation + Defender for Cloud
```

---

## Comparison

| Compare | Difference |
| --- | --- |
| ConfigMgr vs. [[Intune]] | On-prem, infrastructure-heavy, Windows-centric vs. cloud-native MDM/MAM across platforms. **Co-management** = both, with workloads shifted independently. |
| ConfigMgr vs. [[Azure Update Manager]] | Hierarchy, phased deployments, orchestration groups, Windows-only updates vs. SaaS, flat, Windows + Linux, Policy-integrated. |
| ConfigMgr vs. [[WSUS]] | Full management suite vs. the update catalog it uses (SUP). |
| SCCM vs. MECM vs. MCM | Same product renamed: System Center CM → Microsoft Endpoint CM → Microsoft Configuration Manager (Intune family). |

---

## AZ-500 Overlap

Not an AZ-500 topic beyond knowing Intune co-management exists.

## What's New in SC-100

- Positioned as the **"traditional enterprise"** patching approach vs. cloud-native AUM.
- Microsoft's modernization path maps ConfigMgr capabilities to Azure-native services ([[Azure Update Manager]], [[Azure Arc]], [[Azure Policy]] machine configuration, [[Microsoft Defender for Cloud]]).
- Legacy exam scenario: integrating Azure Automation Update Management with ConfigMgr (see [[Azure Update Manager]]).

---

## Exam Tips

- If a scenario says updates are **not delivered by ConfigMgr** and there's **no WSUS**, the machine's agent needs **Microsoft Update** access.
- "Migrate to cloud management without disruption" → co-management.
- "Low-bandwidth branch needs locally cached updates at scale" → ConfigMgr distribution points.
- "Mobile devices / BYOD" → Intune, not ConfigMgr.

## Common Exam Confusion

- **"Configure ConfigMgr to manage security updates"** is a distractor when the goal is to hand updates to Update Management/AUM.
- **Hybrid-join**: an Intune/Conditional Access requirement, not a ConfigMgr–Update Management prerequisite.

---

## Keywords

- SCCM / MECM / MCM, Intune family
- Software Update Point (SUP), WSUS, SUSDB
- Automatic Deployment Rules (ADR), phased deployment, orchestration groups
- Distribution Points, Software Center
- Co-management, tenant attach
- Central Administration Site, Primary/Secondary site

---

## Related Services

- [[Intune]]
- [[WSUS]]
- [[Azure Update Manager]]
- [[Security Updates and Patch Management]]
- [[Azure Arc]]
- [[Entra ID]]
- [[Securing Server and Client Endpoints]]

---

## References

- [What is Configuration Manager?](https://learn.microsoft.com/en-us/intune/configmgr/core/understand/introduction) — Microsoft Learn
- [Patching guidance: Configuration Manager to Azure Update Manager](https://learn.microsoft.com/en-us/azure/update-manager/guidance-migration-azure) — Microsoft Learn
