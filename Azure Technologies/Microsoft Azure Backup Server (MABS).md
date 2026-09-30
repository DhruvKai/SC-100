---
tags:
  - sc100
type: service
domain:
  - security-strategy
aliases:
  - MABS
  - Azure Backup Server
  - Microsoft Azure Backup Server
---

# Microsoft Azure Backup Server (MABS)

## Purpose

A dedicated Windows server, derived from System Center DPM, that makes **application-consistent** backups of on-prem (or Azure) workloads to local disk. It then tiers a second copy to an Azure Recovery Services vault via the [[MARS Agent]].

---

## Why Architects Choose It

- **Application-consistent** backup of SQL Server, Exchange, SharePoint, Hyper-V/VMware/Azure Local VMs and Windows clients from one console. The [[MARS Agent]] alone can't do this.
- **Disk-to-disk-to-cloud**: fast local restores from the first copy, with an offsite Azure copy for ransomware/BCDR (see [[Ransomware Resiliency and BCDR]]).
- **No System Center license** required, unlike DPM.
- Vault protections apply: immutable vaults, soft delete, security PIN for hybrid backups, [[Resource Guard]] / MUA.

---

## Key Facts

| Topic | Detail |
| --- | --- |
| Version | **MABS v4** (Windows Server 2019/2022, SQL 2019/2022 local instance) |
| Placement | On-prem MABS protects on-prem workloads. MABS **in an Azure VM** protects Azure VMs (same domain). |
| Domain | **Must be domain-joined**. It can't move domains later. |
| Dedicated server | Not on a DC, Exchange server, SCOM server, cluster node, Server Core or Hyper-V Server |
| Storage | Local disk is the **first, mandatory copy**. The Azure vault holds the second copy. |
| Azure leg | Uses the **MARS agent** internally, with a passphrase (≥16 characters) to encrypt data |
| Agent | DPM protection agent on protected servers |
| Tape | **Not supported** (DPM supports tape) |
| Connectivity loss | After more than 15 days disconnected, backups stop. A deprovisioned subscription deletes Azure recovery points. |

---

## When to Use

- On-prem SQL/Exchange/SharePoint/Hyper-V/VMware that needs app-consistent backup plus an Azure offsite copy.
- Protecting Linux VMs at hypervisor level (Hyper-V/VMware), where MARS can't help.
- Organizations without System Center licensing.

## When NOT to Use

- Files/folders/system state only → [[MARS Agent]] (no server needed).
- Azure IaaS VMs where native Azure VM backup works → Azure Backup VM extension.
- Tape required, or System Center integration → DPM.

---

## Comparison

| Compare | Difference |
| --- | --- |
| MABS vs. [[MARS Agent]] | Server, app-consistent workloads, local copy + Azure vs. agent-only files/folders/system state straight to the vault |
| MABS vs. DPM | Same engine. MABS: free server, no tape, no System Center integration. DPM: System Center license, tape support. |
| MABS vs. Azure VM backup | On-prem/hypervisor workloads vs. native Azure VM snapshots |

---

## AZ-500 Overlap

Not covered in AZ-500 beyond Recovery Services vault security features.

## What's New in SC-100

- Choose the hybrid backup mechanism by workload type and application consistency, as part of ransomware BCDR design.

---

## Exam Tips

- "Application-consistent backup of on-prem SQL/Exchange/SharePoint/Hyper-V" → **MABS or DPM**.
- "No System Center license" → MABS. "Tape" → DPM.
- "Files/folders only, no server" → MARS agent.

## Common Exam Confusion

- **MABS vs. MARS**: MABS *uses* MARS for the Azure leg. They aren't alternatives for app workloads.

---

## Keywords

- Microsoft Azure Backup Server, MABS v4
- Disk-to-disk-to-cloud, Modern Backup Storage
- Application-consistent backup
- DPM protection agent, Recovery Services vault
- No tape, no System Center integration

---

## Related Services

- [[MARS Agent]]
- [[Ransomware Resiliency and BCDR]]
- [[Resource Guard]]
- [[Securing Active Directory Domain Services (AD DS)]]

---

## References

- [Use Azure Backup Server to back up workloads](https://learn.microsoft.com/en-us/azure/backup/backup-azure-microsoft-azure-backup) — Microsoft Learn
- [MABS protection matrix](https://learn.microsoft.com/en-us/azure/backup/backup-mabs-protection-matrix) — Microsoft Learn
