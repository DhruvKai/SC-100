---
tags:
  - sc100
type: service
domain:
  - infrastructure
aliases:
  - ARG
  - Resource Graph Explorer
---

# Azure Resource Graph

## Purpose

A KQL query engine for exploring Azure resources, and Defender for Cloud posture data, **at scale across subscriptions and management groups**.

---

## Why Architects Choose It

- **Fast, cross-subscription inventory** without per-subscription API calls. The basis for governance dashboards and workbooks.
- Holds **Defender for Cloud assessment data** (`securityresources` table), so posture can be queried, grouped and exported programmatically.
- Every recommendation has **Open query**, which returns either affected resources or security findings.
- Feeds [[Azure Update Manager]] reporting, Policy compliance views and custom workbooks.

---

## When to Use

- Counting, filtering or grouping resources or findings across many subscriptions.
- Building custom posture/compliance dashboards (Workbooks).
- Asset inventory and change history.

## When NOT to Use

- Understanding **how a weakness could be exploited** (MITRE ATT&CK context) → open the [[Defender for Cloud Recommendations|recommendation]].
- Fixing a finding → Quick fix / remediation.
- Log/event analysis → Log Analytics / [[Microsoft Sentinel]] (ARG queries resource state, not logs).
- Discovering assets **outside** your Azure view (shadow IT) → [[External Attack Surface Management (EASM)]].

---

## Comparison

| Compare | Difference |
| --- | --- |
| ARG vs. Log Analytics | Current resource **state/config** vs. **log and event** data |
| ARG vs. [[Defender for Cloud Recommendations\|recommendation details]] | Raw query results at scale vs. contextual finding (MITRE, remediation, risk) |
| ARG vs. [[Azure Policy]] | Query what exists vs. evaluate/enforce what's allowed |

---

## AZ-500 Overlap

Using Resource Graph Explorer and "Open query" from recommendations is AZ-500 level.

## What's New in SC-100

- Treat ARG as the **reporting/automation layer** of posture management across hybrid/multicloud, not as the analysis answer.

---

## Exam Tips

- "Query posture data at scale across subscriptions" → ARG.
- "Determine MITRE ATT&CK tactics for a finding" → **not** ARG. Use the recommendation.

## Common Exam Confusion

- ARG vs. recommendation details (see Comparison).

---

## Keywords

- Azure Resource Graph Explorer, KQL
- `resources`, `securityresources` tables
- Open query (affected resources / security findings)
- Cross-subscription, management group scope

---

## Related Services

- [[Defender for Cloud Recommendations]]
- [[Microsoft Defender for Cloud]]
- [[Security Posture Assessments]]
- [[Azure Policy]]
- [[Azure Update Manager]]
- [[External Attack Surface Management (EASM)]]

---

## References

- [Azure Resource Graph overview](https://learn.microsoft.com/en-us/azure/governance/resource-graph/overview) — Microsoft Learn
- [Review recommendations in Azure Resource Graph](https://learn.microsoft.com/en-us/azure/defender-for-cloud/review-security-recommendations#review-recommendations-in-azure-resource-graph) — Microsoft Learn
