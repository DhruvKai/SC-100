---
tags:
  - sc100
type: concept
domain:
  - apps-data
aliases:
  - Transport rules
  - Exchange mail flow rules
  - Exchange transport rules
---

# Mail Flow Rules

## Purpose

Exchange Online **transport rules** that inspect messages **in transit** and act on them: encrypt, block, redirect, add disclaimers, bypass filtering, and more.

---

## Why architects choose it

- **Org-wide, server-side policy** for email. It doesn't depend on users choosing to encrypt or on client settings.
- The standard way to **apply [[Microsoft Purview Message Encryption]] automatically** by condition (subject keyword, recipient outside org, attachment, sensitive content).
- Rich **conditions + exceptions + actions** cover many messaging policies (disclaimers, approvals, blocking, header changes).

---

## When to use

- Automatically **encrypt** outbound email matching conditions (after Azure RMS is active).
- Add **disclaimers**, prefix subjects, add Bcc, **require approval**, **reject/redirect** messages.
- **Bypass filtering** for specific trusted internal senders (scanners, fax). Never for all internal mail.
- **Test** a policy before enforcing it (test mode, with or without **Policy Tips**).

---

## When NOT to use

- **As the first step of an OME rollout.** Rules need **Azure RMS active** first.
- For **at-rest** classification of documents → [[Sensitivity Labels]].
- For broad sensitive-information protection across Exchange, SharePoint, OneDrive, Teams and endpoints → **Purview DLP** (see [[Data Classification and Protection]]).
- For **inbox** organization by the user. Those are Outlook **Inbox rules**, which act **after delivery**.

---

## Components

| Component | Purpose | Logic |
| --- | --- | --- |
| **Conditions** | Identify messages (headers: To/From/Cc; properties: subject, body, attachments, size, classification) | Multiple conditions = **AND**. One condition with multiple values = **OR**. |
| **Exceptions** | Messages the actions **shouldn't** apply to. **Exceptions override conditions.** | Multiple exceptions = **OR** |
| **Actions** | What to do: reject, delete, redirect, add recipients, prefix subject, disclaimer, **apply OME** | Multiple actions = **AND** (all applied). Some actions (delete silently, forward) stop or limit further actions. |
| **Properties** | Priority, mode, severity, dates, stop processing, etc. | — |

> ⚠️ A rule with **no conditions or exceptions** applies to **all messages**. A delete action would delete all inbound and outbound mail org-wide.

### Properties

| Property (EAC) | PowerShell | Notes |
| --- | --- | --- |
| **Priority** | `Priority` | Rules processed in order. **0 = highest.** Older rules default higher. |
| Severity | `SetAuditSeverity` | DoNotAudit / Low / Medium / High (incident report + message tracking) |
| **Mode** | `Mode` | **Enforce**, **Test with Policy Tips**, **Test without Policy Tips** |
| Activate/Deactivate dates | `ActivationDate` / `ExpiryDate` | Time-bound rules |
| On/off | `Enabled`, `Enable-/Disable-TransportRule` | Disable without deleting |
| Defer if processing fails | `RuleErrorAction` | Default: ignore. Option: resubmit. |
| Match sender address in | `SenderAddressLocation` | Header, envelope or both |
| **Stop processing more rules** | `StopRuleProcessing` | An action that looks like a property |

---

## How rules are applied

- **All messages except NDRs** are evaluated against enabled rules in **priority order**.
- New or changed rules take **up to 30 minutes** to apply.
- **No history**: you can't revert a rule to a previous state.
- **Not processed**: system-generated messages such as Exchange **NDRs**, arbitration mailbox messages (approval requests), **journal reports**.

| Message type | Rule can… |
| --- | --- |
| Regular, clear-signed, anonymous, read reports | Fully process |
| **OME-encrypted** | Always read **envelope headers**. Inspect/modify content only with **transport decryption** enabled. Can also auto-**decrypt**. |
| **S/MIME-encrypted** | **Envelope headers only.** No content inspection or modification. |
| **RMS-protected** (AD RMS / Azure RMS) | Envelope headers. Content only if `TransportDecryptionSetting` = **Mandatory/Optional** (`Set-IRMConfiguration`). |

---

## Exam Scenario — OME first step

Medical records to Gmail/Yahoo!, recommend Purview Message Encryption, **what first?** → **Verify Azure RMS is activated**. *Define mail flow rules in the EAC* is **not first**. Rules are how OME gets applied **after** the RMS prerequisite is confirmed. Full scenario in [[Microsoft Purview Message Encryption]].

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Mail flow rules vs. Inbox rules** | Mail flow = **in transit**, admin, org-wide, richer conditions. Inbox = **after delivery**, per user. |
| **Mail flow rules vs. Purview DLP** | Mail flow = Exchange-only messaging policy. DLP = cross-workload sensitive-info protection with policy tips and endpoint/Teams coverage. |
| **Mail flow rule vs. OME** | Rule = **trigger** (when). OME = **protection** (how). |
| **Exceptions vs. conditions** | Exceptions **override** conditions. Multiple exceptions = OR, multiple conditions = AND. |
| **Test with vs. without Policy Tips** | Both don't affect delivery. "With" also shows senders a Policy Tip in Outlook. |

---

## AZ-500 overlap

Not covered in AZ-500, which is Azure-focused. SC-100 expects it as part of **M365 data protection** design, especially the OME dependency chain.

---

## What's new in SC-100

- Mail flow rules as an **enforcement point** in the email protection design (encrypt/decrypt, bypass filtering safely).
- Know the **ordering**: Azure RMS → OME → mail flow rules.

---

## Exam Tips

- AND across conditions, OR within one condition's values, OR across exceptions, AND across actions.
- Priority **0 = highest**. Use **Stop processing more rules** to prevent later rules applying.
- Up to **30 minutes** for changes to apply.
- S/MIME → headers only. RMS/OME content inspection needs **transport decryption**.
- Encryption action: **Apply Office 365 Message Encryption and rights protection**.

---

## Keywords

- Mail flow rules, transport rules, Exchange admin center (EAC), New-TransportRule
- Conditions, exceptions, actions, properties, priority, mode, Policy Tips
- Stop processing more rules, RuleErrorAction, SenderAddressLocation, SetAuditSeverity
- Transport decryption, TransportDecryptionSetting, S/MIME envelope headers
- Apply Office 365 Message Encryption and rights protection
- Bypass spam filtering for trusted internal senders (scanners/fax)

---

## Related Services

- [[Microsoft Purview Message Encryption]]
- [[Data Classification and Protection]]
- [[Sensitivity Labels]]
- [[Securing Microsoft 365]]
- [[Purview]]

---

## References

- [Mail flow rules (transport rules) in Exchange Online](https://learn.microsoft.com/en-us/exchange/security-and-compliance/mail-flow-rules/mail-flow-rules) — Microsoft Learn (read 2026-10-20)
- [[Exam Objectives]]
