---
tags:
  - sc100
type: service
domain:
  - apps-data
  - ops-identity-compliance
aliases:
  - Microsoft Power Automate
  - Power Platform
status: needs-verification
---

# Power Automate

> **In one line:** Power Automate is the **Power Platform low-code tool for business users** to automate tasks across Microsoft 365 and SaaS apps. For **security automation**, the exam answer is [[Logic Apps]].

## Purpose

Citizen-developer workflow automation: cloud flows (connectors), desktop flows (RPA) and approvals, governed through the Power Platform admin center.

---

## Key Facts

| Item | Detail |
| --- | --- |
| **Flow types** | Cloud flows (automated / instant / scheduled), desktop flows (RPA), business process flows |
| **Connectors** | Hundreds of Microsoft and third-party connectors; runs using the maker's **connections** |
| **Environments** | Containers for apps/flows/data; separate dev/test/prod |
| **Power Platform DLP (data) policies** | Classify connectors as **Business / Non-business / Blocked** to stop data mixing between them |
| **Managed Environments** | Extra governance: sharing limits, usage insights, solution checker |

## Security Concerns for Architects

- **Shadow automation**: users can move data to personal SaaS via connectors. Power Platform DLP policies are the control.
- Flows run with the **maker's credentials**. Ownership and sharing need governance.
- Tenant-wide policy, environment strategy and [[Conditional Access]] for Power Platform access.

---

## When to Use

- Business-user productivity automation in Microsoft 365 (approvals, notifications).

## When NOT to Use

- Sentinel playbooks or Defender for Cloud workflow automation → [[Logic Apps]].
- Code-heavy event processing → [[Azure Functions]].

---

## Exam Tips

- **Security automation = Logic Apps**, not Power Automate.
- **"Stop flows moving business data to non-business connectors"** → **Power Platform DLP policy**, not Purview DLP.

## AZ-500 Overlap

- Not AZ-500 content.

## What's New in SC-100

- Power Platform is part of the **SaaS / citizen-development attack surface**. Govern it with environments, connector DLP and Conditional Access.

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Power Automate vs. Logic Apps** | Power Platform citizen tool vs. Azure resource (ARM, RBAC, managed identity, VNet); security playbooks use Logic Apps |
| **Power Platform DLP vs. [[Purview]] DLP** | Controls which **connectors** can share data vs. detects and blocks **sensitive content** |
| **Power Automate vs. [[Azure Automation]]** | Low-code SaaS flows vs. PowerShell/Python runbooks for infrastructure |

## Keywords

- Power Platform, cloud flow, desktop flow, RPA, connectors, environments
- Power Platform DLP / data policies, Business / Non-business / Blocked, Managed Environments
- Citizen developer, maker connections

## Related Services

- [[Logic Apps]] · [[Azure Functions]] · [[Azure Automation]] · [[Purview]]
- [[SaaS Application Discovery and Control]] · [[Conditional Access]]

## References

- [Power Automate documentation](https://learn.microsoft.com/en-us/power-automate/): Microsoft Learn
- [Data policies (DLP) in Power Platform](https://learn.microsoft.com/en-us/power-platform/admin/wp-data-loss-prevention): Microsoft Learn

## Verification Flag

- Power Platform has been renaming "DLP policies" to "data policies" in the admin center. Check which name current Learn pages and the exam use.
