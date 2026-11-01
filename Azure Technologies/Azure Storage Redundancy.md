---
tags:
  - sc100
type: service
domain:
  - infrastructure
  - security-strategy
aliases:
  - Storage Redundancy
  - LRS
  - ZRS
  - GRS
  - GZRS
  - Backup Storage Redundancy
  - Recovery Services vault redundancy
  - Storage replication type
---

# Azure Storage Redundancy (LRS / ZRS / GRS / GZRS)

## Purpose

Choose how many copies of data Azure keeps and **where**: one datacenter, three availability zones, or a second region. For backup, that choice is the Recovery Services vault's **storage replication type**.

---

## Why Architects Choose It

- Redundancy is a **resiliency vs. data residency vs. cost** trade-off, and the exam tests it as a requirements-matching problem.
- **Data residency** ("keep it in this region") rules out every geo option (GRS, GZRS, RA-*), because they copy data to a second region.
- **Availability-zone failure** ("datacenter outage", "replicate across availability zones") rules out LRS, because all three LRS copies sit in one datacenter.
- Backup redundancy is part of [[Ransomware Resiliency and BCDR]]. The vault copy has to survive the same failure the source does.

---

## The Four Options

| Option | Copies | Where | Survives | Durability (per year) | Data leaves region? |
| --- | --- | --- | --- | --- | --- |
| **LRS** (locally redundant) | 3, synchronous | **One datacenter** in the primary region | Drive, server, rack failure | ≥ 11 nines | No |
| **ZRS** (zone-redundant) | 3, synchronous | **Three availability zones** in the primary region | **Datacenter/zone** failure | ≥ 12 nines | **No** |
| **GRS** (geo-redundant) | 6 = LRS primary + async LRS copy | Primary region + **paired secondary region** | **Region** outage | ≥ 16 nines | **Yes** |
| **GZRS** (geo-zone-redundant) | 6 = ZRS primary + async LRS copy | 3 zones in primary + paired secondary region | Zone **and** region failure | ≥ 16 nines | **Yes** |

- **RA-GRS / RA-GZRS** add **read access** to the secondary region without a failover.
- Geo-replication is **asynchronous**, so there's an RPO (data written just before a regional outage can be lost).
- **Secondary region = the Azure paired region.** Usually it's in the same geography, but not always: **Brazil South pairs with South Central US**, so data leaves the country.

---

## Recovery Services Vault Specifics

| Fact | Detail |
| --- | --- |
| Options offered | **GRS** (default), **LRS**, **ZRS**. **GZRS isn't offered** for a Recovery Services vault. |
| Default | **GRS** |
| When to set it | **Before configuring any backup.** Once a backup is configured, the setting is locked. |
| Changing later | Create a new vault and either stop protection and delete the data (destructive), or stop protection with retained data, re-register in the new vault and pay for both. |
| Cross Region Restore (CRR) | **GRS vaults only.** Restores into the **paired secondary region**, e.g. for DR drills or a primary-region disaster. Extra cost, and items take up to 48 h to appear in the secondary region. Once protection starts, a CRR vault can't go back to plain GRS or LRS. |
| Vault region | Must be in the **same region as the data source**. Use one vault per region. |
| On-prem sources | [[MARS Agent]] and [[Microsoft Azure Backup Server (MABS)]] backups land in the vault, so **the vault's** redundancy setting is the one that matters. |
| Azure Files snapshots | Snapshots live in the file share's own storage account, so vault redundancy doesn't apply to them (it only applies to vaulted file-share backup). |

**Microsoft Learn's own recommendations (source of the exam question):**

| If… | Choose |
| --- | --- |
| Azure is your **primary** backup storage endpoint | **GRS** (keep the default) |
| Azure is **not** your primary backup endpoint (e.g. it's a secondary copy) | **LRS** to cut cost |
| You need **data availability without downtime in a region, guaranteeing data residency** | **ZRS** |

---

## When to Use

- **LRS**: non-critical, reconstructable or dev/test data, or a secondary backup copy where cost matters most.
- **ZRS**: resilience to a zone outage **plus** data residency in one region (the regulated in-region scenario).
- **GRS**: protection from a region-wide outage. Required for **Cross Region Restore**.
- **GZRS**: storage accounts that need both zone and region resilience (highest availability and durability).

## When NOT to Use

- **GRS/GZRS** when the requirement says **data residency / same region**: the secondary copy leaves the region.
- **LRS** when the requirement mentions **availability zones** or datacenter failure.
- **ZRS** when the requirement is surviving a **regional** disaster or restoring in another region.
- **GZRS for a Recovery Services vault**: not an available option.
- In a region **without availability zones**: ZRS/GZRS aren't available there.

---

## Architecture Decisions

```
Must data stay in ONE region (data residency)?
  ├─ YES → Must it survive a zone/datacenter failure?
  │          ├─ YES → ZRS
  │          └─ NO  → LRS (cheapest; rack/drive failures only)
  └─ NO  → Must it survive a REGIONAL outage?
             ├─ YES → Need to restore backups in the secondary region?
             │          ├─ YES → GRS + Cross Region Restore (vault)
             │          └─ NO  → Storage account: GZRS (zones + region)
             │                   Recovery Services vault: GRS (GZRS not offered)
             │          Need to READ the secondary without failover? → RA-GRS / RA-GZRS
             └─ NO  → LRS (or ZRS if zone resilience matters)
```

**Keyword → answer:**

| Scenario wording | Answer |
| --- | --- |
| "availability zones" + "same region" / "data residency" | **ZRS** |
| "region-wide outage", "secondary region", "paired region" | **GRS** |
| "restore in the secondary region", "DR drill in another region" | **GRS + Cross Region Restore** |
| "zone **and** region failure" (storage account) | **GZRS** |
| "lowest cost", "rack and drive failures", "single datacenter" | **LRS** |
| "read from the secondary without failover" | **RA-GRS / RA-GZRS** |

---

## Exam Scenario (got wrong)

> Hybrid estate. Azure Backup protects Azure and on-prem Windows VMs. On-prem backups are incremental and use **LRS**. An audit requires that on-prem VMs be protected and **replicated across availability zones to guarantee data residency and resiliency in the same region**.

| Option | Verdict | Why |
| --- | --- | --- |
| **Configure ZRS for the recovery vault** | ✅ | Three zones, **one region**. Microsoft Learn names ZRS for "availability without downtime in a region, guaranteeing data residency". |
| Configure GRS | ❌ (my pick) | GRS copies data to a **second region**, which breaks "same region". Its primary copy is also LRS (one datacenter), so it doesn't even give zone replication. |
| Configure GZRS | ❌ | Has zones, but also copies to a secondary region, which breaks residency. Not offered for Recovery Services vaults anyway. |
| Leave as LRS | ❌ | One datacenter, so no availability-zone replication. |

**Why GRS was a trap:** "more redundancy" feels safer, but the audit asks for **in-region** resilience. Read the scope words: **zones = ZRS, regions = GRS**.

**Ignore the noise:** "incremental backups" has nothing to do with redundancy.

> ⚠️ The practice-test explanation's last paragraph has region and zone redundancy **backwards**. Zone redundancy protects against a **datacenter** failure inside one region. Region (geo) redundancy protects against a disaster that takes out a **whole region**.

---

## Comparison

| Compare | Difference |
| --- | --- |
| **LRS vs. ZRS** | One datacenter vs. three availability zones. Both keep data in one region. |
| **ZRS vs. GRS** | Zone failure + residency vs. region failure + data leaves the region. GRS's primary copy is LRS, so it has no zone protection in the primary region. |
| **GRS vs. GZRS** | Primary copy is LRS vs. ZRS. Both replicate to the paired region. |
| **GRS vs. RA-GRS** | Secondary readable only after failover vs. readable at any time. |
| **Storage redundancy vs. [[Azure Site Recovery\|Site Recovery]]** | Data copies at the storage layer vs. VM replication and orchestrated failover. |
| **Vault redundancy vs. immutability / soft delete** | Survives infrastructure failure vs. survives malicious or accidental deletion. You need both for [[Ransomware Resiliency and BCDR]]. |

---

## AZ-500 Overlap

AZ-500 covers creating Recovery Services vaults, soft delete and backup RBAC. Storage account redundancy is AZ-104 territory and assumed knowledge.

## What's New in SC-100

- Redundancy as a **requirements-matching decision**: residency vs. resiliency vs. cost.
- Backup storage redundancy as part of a **BCDR / ransomware resiliency** design, not a portal setting.
- Recognizing when a "more redundant" answer **violates** a residency requirement.

---

## Exam Tips

- **"Availability zones" + "same region" → ZRS.** Every time.
- **Recovery Services vault default = GRS.** Set redundancy **before** the first backup is configured.
- **Cross Region Restore → GRS only.**
- **Vault options = LRS / ZRS / GRS.** If GZRS appears for a vault, it's a distractor.
- Data residency + geo-redundancy = contradiction (watch for paired-region exceptions like Brazil South).

## Common Exam Confusion

| Confusion | Resolution |
| --- | --- |
| "Zone" vs. "region" | Zone = datacenter(s) **inside** a region. Region = the whole geographic area. |
| GRS = "most redundant, so safest" | Not if the requirement is in-region residency. |
| GZRS = "ZRS plus extra" | It's ZRS **plus a copy in another region**, so it fails residency. |
| LRS durability (11 nines) = "highly available" | Durable, but a datacenter outage still makes it unavailable. |

---

## Keywords

- Locally redundant / zone-redundant / geo-redundant / geo-zone-redundant storage
- Storage replication type, Backup Configuration, vault properties
- Data residency, same region, availability zones
- Paired region, secondary region, Cross Region Restore (CRR)
- Read-access geo-redundant (RA-GRS / RA-GZRS)
- 11 / 12 / 16 nines durability
- Default GRS, set before configuring backup

---

## Related Services

- [[Ransomware Resiliency and BCDR]]
- [[MARS Agent]]
- [[Microsoft Azure Backup Server (MABS)]]
- [[Resource Guard]]
- [[Azure Well-Architected Framework (WAF)]]: Reliability pillar
- [[Shared Responsibility Model]]
- [[Compliance and Privacy]]: data residency requirements
- [[Azure Policy]]: audit/enforce allowed redundancy SKUs
- [[Key Vault]]

---

## References

- [Create and configure a Recovery Services vault](https://learn.microsoft.com/en-us/azure/backup/backup-create-recovery-services-vault) — Microsoft Learn
- [Azure Storage redundancy](https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy) — Microsoft Learn
- [Azure region pairs](https://learn.microsoft.com/en-us/azure/reliability/cross-region-replication-azure) — Microsoft Learn
