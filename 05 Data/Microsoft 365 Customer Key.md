---
tags:
  - sc100
type: service
domain:
  - apps-data
aliases:
  - Customer Key
  - Purview Customer Key
  - Microsoft 365 service encryption with Customer Key
  - Data encryption policy
  - DEP
status: needs-verification
---

# Microsoft 365 Customer Key

## Purpose

Lets an organization supply and control the **root keys** that Microsoft 365 uses to encrypt **data at rest** in Microsoft datacenters (Exchange, SharePoint/OneDrive, Teams and other workloads), on top of Microsoft's default service encryption.

---

## Why Architects Choose It

- **Compliance requirement to manage (and be able to revoke) encryption keys** for M365 data at rest.
- **Availability-safe**: two keys in two subscriptions, plus a Microsoft **availability key**, so Microsoft 365 features (search, eDiscovery, Copilot) keep working. That's unlike [[Double Key Encryption (DKE)]].
- **Exit control**: revoking keys starts the **data purge path** when leaving Microsoft 365.
- Keys live in **[[Key Vault]] (Premium, HSM-backed)** or **Managed HSM**, both customer-controlled.

---

## Setup Order (verified on Learn, 2025)

```
PREREQUISITES
  1. Licensing (E5 / E5 Compliance / Information Protection & Governance add-on)
  2. Two NEW paid Azure subscriptions (same Entra tenant), different admins per key
  3. Register service principals (Customer Key Onboarding app, M365DataAtRestEncryption, Exchange Online)
  4. Key Vault per workload in EACH subscription (Premium recommended):
       soft delete + purge protection (90 days) at creation,
       role "Key Vault Crypto Service Encryption User" for the M365 app (wrapKey/unwrapKey/get)
     (or 2 Managed HSMs total, purge protection on)
  5. Create or IMPORT an RSA key in each vault, no expiration date, back it up, record the key URIs
  ↓
FIRST STEP AFTER PREREQUISITES
  6. Run the Customer Key Onboarding Service
       Install-Module M365CustomerKeyOnboarding
       New-CustomerKeyOnboardingRequest -Scenario MDEP|EXO -OnboardingMode Validate → Enable
  ↓
THEN
  7. Create and assign Data Encryption Policies (DEPs) in Exchange Online PowerShell
       multi-workload DEP (tenant-wide) · Exchange mailbox DEPs (≤50, one per mailbox)
       · SharePoint/OneDrive DEP (per geo)
```

- **SharePoint and OneDrive** don't use the onboarding service. After vault prerequisites, you create the SharePoint DEP directly. Special clouds (GCC-H/DoD/21Vianet) onboard via Microsoft Support.
- The onboarding request needs the **key URIs**, so keys must already exist in Key Vault **before** onboarding.

---

## When to Use

- Regulation requires the **customer to manage/revoke** M365 at-rest encryption keys.
- You want key control **without** losing M365 service features.

## When NOT to Use

- Microsoft must be **technically unable** to read specific content → [[Double Key Encryption (DKE)]] (for a small, highly sensitive subset).
- Per-item protection that travels with the file/email → [[Sensitivity Labels]] / Azure RMS encryption.
- Azure resources (Storage, SQL, disks) → Azure **CMK** via Key Vault, not Microsoft 365 Customer Key.
- Controlling Microsoft engineer **access** (not keys) → **Customer Lockbox**.

---

## Exam Scenario (got wrong): "What first, after prerequisites?"

> M365 E5. Office apps create PII and users store data in OneDrive. Compliance requires the company to **manage the keys M365 uses to encrypt data at rest**. What should the company do **first after completing prerequisites**?

| Option | Verdict | Why |
| --- | --- | --- |
| **Run the Customer Key Onboarding Service** | ✅ | The first action after the Azure prerequisites. It **validates** the subscriptions, vaults and keys, then **enables** Customer Key for the tenant (cmdlets in Exchange/Azure PowerShell). Without it, no DEP can use your keys. |
| Import the company's keys into an Azure key vault | ❌ (my pick) | Key creation/import is part of the **prerequisites** (the onboarding request needs the key URIs), so it isn't the next step. *(The practice explanation says keys are imported "after Customer Key has been approved". That's the older process. Current Learn puts keys before onboarding. Either way, it's not the answer.)* |
| Generate keys and configure Double Key Encryption (DKE) | ❌ | A **different product**: per-label, client-side double encryption where Microsoft can't decrypt. It doesn't encrypt M365 service data at rest and breaks search/eDiscovery for that content. |
| Modify the Purview service encryption configuration | ❌ | Assigning **DEPs** (the "service encryption configuration") comes **after** onboarding. You can't point M365 at keys that aren't onboarded. |

**Trap:** "keys" in the requirement pulls you toward the Key Vault option. But "**after completing prerequisites**" means the vault and keys are **done**. The next gate is **onboarding**. Then DEPs.

---

## Comparison

| Compare | Difference |
| --- | --- |
| **Microsoft-managed keys vs. Customer Key** | Default service encryption (BitLocker + service encryption, Microsoft holds keys) vs. customer-supplied root keys in Key Vault, revocable. |
| **Customer Key vs. [[Double Key Encryption (DKE)]]** | Service-side at-rest encryption, all M365 features keep working vs. client-side double encryption of labelled content, Microsoft **can't** decrypt, so features break. |
| **Customer Key vs. BYOK for Azure Information Protection / RMS tenant key** | Encrypts the **service's stored data** vs. the key behind **sensitivity label / RMS** protection of items. |
| **Customer Key vs. Customer Lockbox** | Controls **keys** vs. controls **Microsoft support access** approvals. |
| **Customer Key DEP vs. Defender for Cloud Apps policies** | Key hierarchy for M365 encryption vs. CASB session/anomaly policies. "Data encryption policy" is a Customer Key term. |
| **Customer Key vs. Azure CMK** | Microsoft 365 workloads vs. Azure resources (Storage, SQL TDE, disks). |

---

## AZ-500 Overlap

Key Vault, HSM-backed keys, soft delete and purge protection, and CMK for Azure services are AZ-500. Customer Key for Microsoft 365 isn't.

## What's New in SC-100

- Picking the right **encryption-key control** for M365: Customer Key vs. DKE vs. sensitivity labels vs. Lockbox.
- Knowing the **onboarding sequence** as a "what first" question.
- Trading key control against M365 feature availability.

---

## Exam Tips

- "Company must **manage keys** used by Microsoft 365 to encrypt **data at rest**" → **Customer Key**.
- "First step after prerequisites" → **Customer Key Onboarding Service**. DEPs come after.
- "Microsoft must never be able to decrypt" → **DKE**, not Customer Key.
- Two subscriptions, two keys, soft delete + purge protection: all prerequisites.

## Common Exam Confusion

- **"Data encryption policy" in a Defender for Cloud Apps question** → distractor. DEPs belong to Customer Key.
- **Import keys vs. onboard**: prerequisite vs. first post-prerequisite action.

---

## Keywords

- Customer Key, service encryption, data at rest, root keys
- Customer Key Onboarding Service, `M365CustomerKeyOnboarding`, `New-CustomerKeyOnboardingRequest`, Validate / Enable
- Data encryption policy (DEP): multi-workload, Exchange mailbox, SharePoint/OneDrive
- Availability key
- Two Azure subscriptions, Premium Key Vault / Managed HSM, soft delete + purge protection (90 days)
- Key Vault Crypto Service Encryption User, M365DataAtRestEncryption

---

## Related Services

- [[Key Vault]]
- [[Double Key Encryption (DKE)]]
- [[Data Classification and Protection]]
- [[Sensitivity Labels]]
- [[Microsoft 365 Licensing]]
- [[Purview]]
- [[Compliance and Privacy]]
- [[SaaS Application Discovery and Control]]: where "data encryption policy" appears as a distractor

---

## References

- [Set up Customer Key](https://learn.microsoft.com/en-us/purview/customer-key-set-up) — Microsoft Learn
- [Overview of Customer Key](https://learn.microsoft.com/en-us/purview/customer-key-overview) — Microsoft Learn
- [What is Microsoft 365 service encryption?](https://learn.microsoft.com/en-us/purview/office-365-service-encryption) — Microsoft Learn

---

## Verification Flag

- The licensing SKUs, the availability-key description and DEP scopes are from training knowledge plus the setup page. Re-check them on the Customer Key overview and licensing pages.
