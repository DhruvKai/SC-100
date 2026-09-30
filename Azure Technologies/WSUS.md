---
tags:
  - sc100
type: service
domain:
  - infrastructure
aliases:
  - Windows Server Update Services
  - WSUS server
---

# WSUS (Windows Server Update Services)

## Purpose

A Windows Server role that downloads Microsoft updates from Microsoft Update and lets admins **approve and distribute** them to machines on the internal network.

---

## Why Architects Choose It

- **Centralized control** of which updates reach which machines (approval, computer groups), without every machine going to the internet.
- The **update catalog under [[Microsoft Configuration Manager]]**: ConfigMgr's Software Update Point *is* a WSUS server.
- An update source **[[Azure Update Manager]] can use**, including for **third-party updates** published into WSUS.
- **Deprecated** (no new features) but still supported, with security/quality fixes. Don't design new strategies around it unless it's needed.

---

## Key Facts

| Topic | Detail |
| --- | --- |
| Hierarchy | **Upstream** server syncs from Microsoft Update. **Downstream** servers sync from upstream. At least one server must reach Microsoft Update. |
| Downstream modes | **Autonomous** (own approvals) vs. **Replica** (inherits upstream approvals/groups) |
| Client config | Group Policy: *Specify intranet Microsoft update service location* → the Windows Update Agent (WUA) points at WSUS |
| Database | SUSDB (WID or SQL Server) |
| Admin | WSUS console + **PowerShell cmdlets**, SHA-256 support |
| Status | **Deprecated** (no new features), supported per lifecycle |
| UUP | Supports Unified Update Platform for Windows 11 (needs IIS MIME types/update) |

---

## When to Use

- Existing on-prem Windows estate that needs internal, approval-gated patch distribution.
- Under ConfigMgr (required for its Software Update Point).
- As an AUM update source for approved or third-party updates.
- Isolated networks with no direct internet on each machine (only the upstream server connects).

## When NOT to Use

- Linux patching (not supported).
- Greenfield cloud/hybrid designs → [[Azure Update Manager]] (servers) and [[Intune]]/Windows Autopatch (clients).
- As the answer when the scenario says **no WSUS exists**. The agent needs Microsoft Update instead.

---

## Comparison

| Compare | Difference |
| --- | --- |
| WSUS vs. Microsoft Update | Internal, admin-approved update server vs. Microsoft's public update service. The Windows agent uses **one or the other**. |
| Microsoft Update vs. Windows Update | Windows Update = OS updates only. Microsoft Update = OS **plus other Microsoft products** (Office, SQL, etc.). |
| WSUS vs. [[Microsoft Configuration Manager]] | Catalog/approval engine vs. full management suite on top of WSUS (targeting, ADRs, distribution points, reporting). |
| WSUS vs. [[Azure Update Manager]] | On-prem distribution server vs. cloud orchestration/compliance service. AUM can *consume* WSUS as its source. |

---

## AZ-500 Overlap

Not covered in depth in AZ-500. Update Management's agent source requirement touched it indirectly.

## What's New in SC-100

- Know WSUS as the "traditional enterprise" option in the patching approaches table ([[Security Updates and Patch Management]]).
- Know that it's **deprecated**, and its role as an AUM source during ConfigMgr modernization.

---

## Exam Tips

- Windows agent prerequisite: **WSUS or Microsoft Update**. Pick based on whether WSUS exists.
- Third-party patches with AUM → must be **published to WSUS**.
- "Legacy Windows, granular control, limited cross-platform" → WSUS/ConfigMgr.

## Common Exam Confusion

- **Configure agent for WSUS vs. give agent Microsoft Update access**: the former only makes sense if a WSUS server exists.
- **Autonomous vs. Replica** downstream mode: own approvals vs. inherited approvals.

---

## Keywords

- Windows Server Update Services, SUSDB
- Upstream/downstream, autonomous/replica mode
- Intranet Microsoft update service location (GPO)
- Windows Update Agent (WUA)
- Deprecated, no new features
- Third-party update publishing

---

## Related Services

- [[Microsoft Configuration Manager]]
- [[Azure Update Manager]]
- [[Security Updates and Patch Management]]
- [[Intune]]
- [[Securing Server and Client Endpoints]]

---

## References

- [Windows Server Update Services (WSUS) overview](https://learn.microsoft.com/en-us/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus) — Microsoft Learn
- [Patching guidance: Configuration Manager to Azure Update Manager](https://learn.microsoft.com/en-us/azure/update-manager/guidance-migration-azure) — Microsoft Learn
