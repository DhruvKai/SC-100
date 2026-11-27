---
tags:
  - sc100
type: service
domain:
  - infrastructure
  - best-practices
aliases:
  - Site Recovery
---

# Azure Site Recovery

> **In one line:** Site Recovery **continuously replicates VMs** to another region (or from on-prem to Azure), so you can **fail over and keep running** when a site goes down.
>
> **Remember:** Site Recovery = keep **running** (DR). [[Ransomware Resiliency and BCDR|Backup]] = get data **back** (restore).

## Purpose

Business continuity / disaster recovery (BCDR) service that replicates workloads and orchestrates failover and failback.

---

## Key Facts

| Item | Detail |
| --- | --- |
| **Scenarios** | Azure → Azure (region or zone), VMware / Hyper-V / physical → Azure |
| **Recovery Services vault** | Holds replication settings (shared resource type with Azure Backup) |
| **Recovery points** | Crash-consistent and app-consistent; define the **RPO** |
| **Recovery plans** | Ordered failover of multi-tier apps, with scripts / [[Azure Automation]] runbooks and manual steps |
| **Test failover** | Into an **isolated network**, with no impact on production or replication |
| **Failover / failback** | Planned (no data loss) or unplanned; re-protect then fail back |
| **Protection** | [[Resource Guard]] multi-user authorization applies to Site Recovery too |
| **User redirection** | Pair with [[Azure Traffic Manager]] **Priority** routing (or Front Door) |

---

## Why Architects Choose It

- Meets **RTO/RPO** targets for regional outages without building a second always-on stack.
- **Test failover** proves the DR plan regularly with no downtime: a [[Azure Well-Architected Framework (WAF)|WAF]] reliability practice.
- Recovery plans make failover repeatable and scripted, not a manual scramble.

## When to Use

- Regional or datacenter outage must not stop the business.
- Migrating on-prem VMs to Azure (replicate, then fail over).
- Zone-to-zone DR inside a region.

## When NOT to Use

- **Ransomware / accidental deletion / corruption** recovery on its own. Replication copies the damage too, so use **immutable backups** ([[Ransomware Resiliency and BCDR]]).
- Long-term retention or point-in-time restore → Azure Backup.
- PaaS services with their own geo-replication (SQL failover groups, [[Azure Storage Redundancy|GRS/GZRS storage]]).

---

## Exam Tips

- **"Fail over to another region and keep running"** → Site Recovery.
- **"Restore after ransomware / deletion"** → Backup (immutable, soft delete, MUA), not Site Recovery.
- **"Validate the DR plan without affecting production"** → **test failover**.
- **"Fail over a multi-tier app in order"** → **recovery plan**.
- **"Redirect users after failover"** → Traffic Manager **Priority** routing.

## AZ-500 Overlap

- AZ-500 touches backup and vault security only lightly. SC-100 asks you to design BCDR as part of resiliency strategy.

## What's New in SC-100

- BCDR is a **security** outcome (availability, ransomware resilience). Design **both** replication (Site Recovery) and immutable backup. They solve different failures.

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Site Recovery vs. Azure Backup** | Continuous replication + failover (RTO in minutes) vs. point-in-time copies to restore (survives corruption/ransomware) |
| **Site Recovery vs. storage redundancy** | VM replication + orchestrated failover vs. data copies at the storage layer ([[Azure Storage Redundancy]]) |
| **Test failover vs. planned failover** | Isolated rehearsal, production untouched vs. real move, primary shut down first |
| **Site Recovery vs. availability zones/sets** | Recover from a **region** loss vs. survive a **datacenter/rack** failure in one region |

## Keywords

- BCDR, disaster recovery, RTO, RPO, replication, failover, failback, re-protect
- Recovery Services vault, recovery plan, test failover, isolated network
- Azure-to-Azure, VMware/Hyper-V/physical to Azure, zone-to-zone
- Traffic Manager Priority routing, multi-user authorization

## Related Services

- [[Ransomware Resiliency and BCDR]] · [[Azure Traffic Manager]] · [[Azure Storage Redundancy]]
- [[Resource Guard]] · [[Azure Automation]] · [[Azure Well-Architected Framework (WAF)]] · [[Architecture Decisions]]

## References

- [About Site Recovery](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-overview): Microsoft Learn
- [Run a test failover](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-test-failover-to-azure): Microsoft Learn
- [Traffic Manager with Site Recovery](https://learn.microsoft.com/en-us/azure/site-recovery/concepts-traffic-manager-with-site-recovery): Microsoft Learn
