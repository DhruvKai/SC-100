---
tags:
  - sc100
type: service
domain:
  - apps-data
aliases:
  - Synapse
  - Synapse Analytics
  - Synapse security baseline
  - Azure Synapse Analytics security baseline
status: needs-verification
---

# Azure Synapse Analytics

## Purpose

Microsoft's analytics workspace (dedicated + serverless SQL pools, Apache Spark pools, Data Explorer pools, pipelines). This note covers **every security technology in its MCSB security baseline**: network, identity, privileged access, data protection, policy, threat detection, logging, backup.

---

## Why architects choose it

- Analytics platforms **concentrate sensitive data** from many sources, so Synapse is a top DSPM / data-exfiltration concern.
- Most security features are **customer-configured and off by default**: managed VNet, private endpoints, CMK, Defender for SQL, auditing, Entra-only auth. Designing them in is the architect's job.
- Two irreversible **creation-time decisions** (managed VNet, double encryption with CMK) must be made up front.

---

## Security profile (from the baseline)

| Attribute | Value |
| --- | --- |
| Product category | Analytics |
| Customer access to host/OS | **No access** (PaaS) |
| Deployable into customer VNet | **True** (Managed workspace VNet) |
| Stores customer content at rest | **True** |

---

## Baseline controls → technologies

| MCSB control | Feature | Supported | Default | Responsibility |
| --- | --- | --- | --- | --- |
| **NS-1** Network segmentation | Virtual Network integration (Managed workspace VNet) | ✅ | Off | Customer |
| NS-1 | NSG support | ✅ | Off | Customer |
| **NS-2** Secure cloud services | Azure Private Link (private endpoints, Private Link Hubs) | ✅ | Off | Customer |
| NS-2 | Disable public network access (toggle + IP firewall rules) | ✅ | Off | Customer |
| **IM-1** Centralized identity | Entra ID auth for data plane | ✅ | Off | Customer |
| IM-1 | Local auth (SQL auth) for data plane | ✅ | Off | Customer (**restrict/disable**) |
| **IM-3** App identities | Managed identities | ✅ | **On** | Microsoft |
| IM-3 | Service principals | ✅ | **On** | Microsoft |
| **IM-7** Conditional access | Conditional Access for data plane | ✅ | Off | Customer |
| **IM-8** Credentials/secrets | Key Vault integration for secrets (linked services, `mssparkutils`) | ✅ | Off | Customer |
| **PA-1** Limit privileged users | Local admin accounts (SQL admin) | ✅ | Off | Customer (emergency use only) |
| **PA-7** Least privilege | **Azure RBAC for data plane** | ❌ | N/A | **Synapse RBAC** handles the data plane |
| **PA-8** Cloud provider support access | Customer Lockbox | ✅ | Off | Customer |
| **DP-1** Discover/classify | Sensitive data discovery & classification (Purview, SQL Data Discovery & Classification) | ✅ | Off | Customer |
| **DP-2** Monitor threats to data | DLP / Advanced Threat Protection (Defender for SQL) | ✅ | Off | Customer |
| **DP-3** Encrypt in transit | TLS | ✅ | **On** | Microsoft |
| **DP-4** Encrypt at rest (default) | Platform-managed keys | ✅ | **On** | Microsoft |
| **DP-5** CMK when required | Data at rest with CMK (workspace double encryption) | ✅ | Off | Customer |
| **DP-6** Key management | Key Vault for keys/secrets/certs | ✅ | Off | Customer |
| **AM-2** Approved services | Azure Policy support | ✅ | Off | Customer |
| **LT-1** Threat detection | Microsoft Defender for SQL | ✅ | Off | Customer |
| **LT-4** Logging | Azure resource logs / SQL auditing | ✅ | Off | Customer |
| **BR-1** Backups | **Azure Backup** | ❌ | N/A | — |
| BR-1 | Service-native backup (dedicated SQL pool restore points) | ✅ | Off | Customer |

> Baseline is based on **MCSB v1.0** and Learn warns it "may contain outdated guidance". See [[Microsoft Cloud Security Benchmark (MCSB)]].

---

## Network security

### Managed workspace VNet (NS-1)

- Chosen **at workspace creation** (Networking tab → *Enable managed virtual network*). It **can't be added or removed later**.
- Benefits:
  - Synapse manages the VNet.
  - **No inbound NSG rules** needed for Synapse management traffic, so misconfigured NSGs can't break the service.
  - **No Spark subnet sizing** for peak load.
  - With **managed private endpoints**, it **protects against data exfiltration**.
- **Data integration + Spark** run inside it. Each Spark cluster gets its **own subnet** (user-level isolation).
- **Dedicated and serverless SQL pools are multitenant** and sit **outside** the managed VNet. Intra-workspace traffic to them uses **auto-created private links**.
- **Managed private endpoints only exist in workspaces with a managed VNet.**

### Data Exfiltration Protection (DEP)

- "Allow outbound data traffic only to approved targets" = **Yes**. Outbound only via **managed private endpoints**.
- Approved targets: same Entra tenant by default. **Add other Entra tenants** explicitly.
- Use the **Managed VNet Integration Runtime (VNET IR)** for pipelines/linked services. A public **Azure IR** reference in a DEP workspace bypasses the model (known issue, UI warning).
- Need public outbound → use a **non-DEP workspace** instead of mixing.

### Public network access + firewall (NS-2)

- **Disable public network access** → connect **only via private endpoints**. Can be changed during or after creation. Firewall rules are kept but **greyed out** and inactive.
- **IP firewall rules** apply when public access is enabled.
- With public access disabled: Git mode still works, but **Publish (Live mode) is blocked**. A **self-hosted IR** still communicates (no private link from SHIR to the control plane).
- **Private Link Hubs** provide private access to **Synapse Studio**.
- **Minimal TLS**: serverless and dev endpoints are **TLS 1.2+**. Workspace dedicated SQL defaults to 1.2+ (can't go below 1.2 after **March 31, 2025**).
- **Connection policy** is fixed at **Default** (Redirect inside Azure, Proxy outside).
- No Azure Policy currently prevents changes to Synapse networking settings.

### NSG (NS-1)

- Use NSGs to restrict and monitor traffic on your subnets (e.g. keep management ports away from untrusted networks).

---

## Identity and privileged access

| Topic | Guidance |
| --- | --- |
| **Entra ID auth** (IM-1) | Make **Entra ID the default** for data plane access. Policy: *An Azure AD administrator should be provisioned for SQL servers*. |
| **SQL auth / local admin** (IM-1, PA-1) | **Restrict or disable**. Keep the SQL admin for **emergency use only**. |
| **Managed identity / service principals** (IM-3) | Enabled by default. The workspace **system-assigned managed identity** accesses storage and Key Vault. |
| **Conditional Access** (IM-7) | Applies to Synapse data plane: location, risky sign-in, managed device. See [[Conditional Access]]. |
| **Secrets** (IM-8) | Store in [[Key Vault]], reference from linked services/`mssparkutils`. Never embed in code. |
| **Customer Lockbox** (PA-8) | Approve or reject Microsoft support access to your data |

### Azure RBAC vs. Synapse RBAC vs. SQL permissions

| Layer | Controls |
| --- | --- |
| **Azure RBAC** | **Management plane**: create/update/delete the workspace, SQL pools, Spark pools, integration runtimes |
| **Synapse RBAC** | **Workspace content**: publish code artifacts, run code on Spark/IR, use credentials/linked services, monitor/cancel jobs. Managed in **Synapse Studio → Manage hub**. |
| **SQL security** | **Primary control for SQL pools** (logins, users, GRANT). Synapse RBAC gives only limited SQL control. |

- Synapse RBAC scopes: **workspace** (top, inherited) → Spark pool, integration runtime, linked service, credential. **Additive** model.
- **Synapse Administrator** assigns roles. The workspace creator gets it automatically. Anyone who can manage **Azure RBAC role assignments** on the workspace can also recover Synapse RBAC.
- Personas: administrators, developers (author but not publish/run on prod), operators (monitor, no code access), security staff (manage endpoints without code/data access).

---

## Data protection

### Encryption

| Layer | Detail |
| --- | --- |
| **In transit** (DP-3) | On by default (Microsoft) |
| **At rest, platform keys** (DP-4) | On by default |
| **Double encryption with CMK** (DP-5) | 2nd layer with a **customer-managed key in Key Vault**. Chosen **at workspace creation** (Security tab) and **can't be changed later**. One key encrypts **all** workspace data: dedicated + serverless SQL pools, Data Explorer pools, Spark pools, pipelines/IR/datasets. |
| Key requirements | **RSA 2048/3072** or **RSA-HSM**. No EC/oct-HSM. Key Vault in the **same region**, **purge protection enabled**. |
| Key permissions | Workspace managed identity needs **Get, WrapKey, UnwrapKey** (access policy "Application-only" or Key Vault RBAC) |
| Activation | System-assigned MI → workspace **Pending** until you grant key access and **activate**. **User-assigned MI** avoids phased activation (not when Key Vault is behind a firewall). |
| Rotation | Synapse **doesn't auto re-encrypt** on new key versions. Switch to a temp key → wait 15–30 min → switch back. **Keep old key versions enabled/unexpired** until all **dedicated SQL pools are online** and re-encrypted. Offline pools can't resume otherwise. |
| **TDE** | Policy: *TDE on SQL databases should be enabled*. In workspaces **without** double encryption, **TDE with service-managed key** can be toggled per dedicated SQL pool. See [[SQL Data Protection (TDE, Ledger, TDS 8.0)]]. |
| Key management (DP-6) | [[Key Vault]]: KEK/DEK hierarchy, rotation/revocation schedule, BYOK from on-prem HSM |

### Classification and threat protection

- **DP-1**: [[Purview]] scanning + **SQL Data Discovery & Classification** on dedicated SQL pools.
- **DP-2 / LT-1**: **Microsoft Defender for SQL** (Advanced Threat Protection: anomalous access, SQL injection), plus M365 or host-based DLP if required. Policy: *Azure Defender for SQL should be enabled…*. See [[Microsoft Defender for Cloud]].

---

## Governance, logging, backup

| Control | Technology |
| --- | --- |
| **AM-2** | [[Azure Policy]] built-ins for Synapse (audit/deny, deploy-if-not-exists) via Defender for Cloud regulatory compliance |
| **LT-4** | **Resource logs** + **SQL auditing** to Log Analytics/Storage/Event Hub → [[Microsoft Sentinel]]. See [[Azure Security Logging]]. |
| **BR-1** | **Azure Backup not supported.** Use **dedicated SQL pool native restore points / geo-backups**. |

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Azure RBAC vs. Synapse RBAC** | Azure RBAC = **manage the workspace resource** (control/management plane). Synapse RBAC = **use workspace content** (publish, run, credentials). Baseline: Azure RBAC for data plane = **not supported**. |
| **Managed VNet vs. customer VNet injection** | Synapse **manages** the VNet. You don't deploy Synapse into your own subnet. Must be enabled **at creation**. |
| **Managed private endpoint vs. private endpoint** | Managed PE = **outbound** from the Synapse managed VNet to data sources (DEP). Private endpoint / Private Link Hub = **inbound** private access to the workspace/Studio. |
| **Disable public access vs. IP firewall** | Disable = private endpoints only, firewall inactive. Firewall = allow-listed public IPs. |
| **Platform keys vs. CMK double encryption vs. TDE** | Platform = default layer 1. CMK = workspace-wide layer 2, set at creation. TDE (service-managed) = per-pool option in non-CMK workspaces. |
| **System- vs. user-assigned MI for CMK** | System-assigned = phased activation (Pending). User-assigned = no phased activation, but not with a firewalled Key Vault. |
| **Azure Backup vs. native restore points** | Azure Backup isn't supported. Use dedicated SQL pool restore points. |

---

## AZ-500 overlap

You know SQL firewall rules, private endpoints, TDE/CMK, Defender for SQL, Entra auth for SQL and auditing. SC-100 adds Synapse-specific architecture: **managed VNet + DEP**, **creation-time irreversibility**, **Synapse RBAC layer**, and using the **MCSB baseline** as a design checklist.

---

## What's new in SC-100

- Using **service security baselines** (MCSB) to design and assess PaaS data services. Exam objective: "data security for Azure SQL, **Azure Synapse Analytics**, Cosmos DB".
- **Exfiltration-resistant analytics**: managed VNet + DEP + approved tenants + VNET IR.
- **Key ownership** with operational risk: CMK rotation can make pools inaccessible if old keys are disabled too early.

---

## Exam Tips

- Managed VNet and CMK double encryption → **only at workspace creation**.
- Managed private endpoints **require** a managed VNet.
- Prevent data exfiltration → **managed VNet + DEP (approved targets only)**.
- Data plane access control → **Synapse RBAC** (+ SQL permissions), **not Azure RBAC**.
- Backup → **not Azure Backup**. Use native restore points.
- CMK key vault → **purge protection** + same region. MI needs **Get/Wrap/Unwrap**.
- Defender coverage → **Defender for SQL**.

---

## Keywords

- Azure Synapse Analytics, workspace, dedicated SQL pool, serverless SQL pool, Apache Spark pool, Data Explorer pool, integration runtime
- MCSB security baseline, NS-1, NS-2, IM-1, IM-3, IM-7, IM-8, PA-1, PA-7, PA-8, DP-1–DP-6, AM-2, LT-1, LT-4, BR-1
- Managed workspace virtual network, managed private endpoints, data exfiltration protection (DEP), approved Entra tenants, VNET IR
- Private Link Hubs, disable public network access, IP firewall rules, minimal TLS 1.2
- Microsoft Entra authentication, SQL authentication, Conditional Access, managed identity, mssparkutils
- Synapse RBAC, Synapse Administrator, Synapse Studio Manage hub, Azure RBAC (management plane)
- Customer Lockbox
- Double encryption, customer-managed key, purge protection, WrapKey/UnwrapKey/Get, workspace activation, user-assigned managed identity, key rotation
- TDE, SQL Data Discovery & Classification, Purview, Defender for SQL, SQL auditing, resource logs
- Restore points, geo-backup, Azure Backup not supported

---

## Related Services

- [[Data Security Posture Management (DSPM)]] — exam objective: data security for Azure SQL / Synapse / Cosmos DB
- [[SQL Data Protection (TDE, Ledger, TDS 8.0)]]
- [[Microsoft Cloud Security Benchmark (MCSB)]]
- [[Private Link]]
- [[Network Security Group]]
- [[Key Vault]]
- [[Conditional Access]]
- [[Identity and Access Management (IAM)]] — Azure RBAC vs. data-plane roles
- [[Microsoft Defender for Cloud]] — Defender for SQL, regulatory compliance
- [[Azure Policy]]
- [[Purview]]
- [[Azure Security Logging]]
- [[Securing IaaS and PaaS Services]]

---

## References

- [Azure security baseline for Azure Synapse Analytics](https://learn.microsoft.com/en-us/security/benchmark/azure/baselines/azure-synapse-analytics-security-baseline) — Microsoft Learn (read 2026-10-20)
- [Managed virtual network — Azure Synapse](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/synapse-workspace-managed-vnet) — Microsoft Learn
- [Azure Synapse Analytics encryption](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/workspaces-encryption) — Microsoft Learn
- [Synapse role-based access control](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/synapse-workspace-synapse-rbac) — Microsoft Learn
- [Azure Synapse connectivity settings](https://learn.microsoft.com/en-us/azure/synapse-analytics/security/connectivity-settings) — Microsoft Learn
- [[Exam Objectives]]

---

## Verification Flag

- The baseline is built on **MCSB v1.0**, and Learn itself warns it "may contain outdated guidance". Re-check against current Synapse docs and MCSB v2 when available.
- The LT-1 guidance text in the baseline mentions "Microsoft Defender for Key Vault" under Synapse, which looks like a copy-paste error. The referenced offering is **Defender for SQL**.
- Microsoft now steers new data warehouse workloads to **Microsoft Fabric Data Warehouse**. Confirm whether exam questions still target Synapse.
