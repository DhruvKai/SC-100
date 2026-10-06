---
tags:
  - sc100
type: concept
domain:
  - ops-identity-compliance
aliases:
  - Threat Intelligence Reports
  - Activity Group Report
  - Campaign Report
  - Threat Summary Report
---

# Defender for Cloud Threat Intelligence Reports

## Purpose

PDF reports linked from a [[Microsoft Defender for Cloud]] security alert that explain the threat behind it — split into **Activity Group**, **Campaign** and **Threat Summary** reports by scope.

---

## Why architects choose it

- Gives incident responders attacker context (who, why, how) **at the alert**, without a separate TI platform.
- Feeds the "understand the attacker" step of IR and resilience planning — attacker motivations and mitigations to prevent the **next** similar attack (see [[Ransomware Resiliency and BCDR]]).
- **Victimology** (industry + geographic prevalence) helps judge whether your own Azure resources are likely targets.
- Choosing the right report type = choosing the right **scope**: one actor, one campaign, or everything.

---

## When to use

- Investigating a Defender for Cloud alert and needing attacker background → open the report from the alert.
- Suspect a specific **threat actor / APT** is targeting you → **Activity Group Report** (actor TTPs).
- Hit by, or investigating, one specific **attack campaign** → **Campaign Report**.
- Need the full picture (actor + campaigns) → **Threat Summary Report**.

---

## When NOT to use

- Ingesting indicators into detection rules at scale → [[Threat Intelligence]] connectors in [[Microsoft Sentinel]] (TAXII / upload API), not PDF reports.
- Org-wide exposure to an active campaign across endpoints/identity/email → **Threat Analytics** in [[Microsoft Defender XDR]].
- Expecting a report for every alert — content varies by alert type and the malware's activity/prevalence.

---

## Report types

| Report | Scope | Pick it when… |
| --- | --- | --- |
| **Activity Group Report** | Deep dive into **attackers**: identity/associations, objectives, **tactics, techniques and procedures (TTPs)** | You suspect a specific **group/APT** — narrow, actor-focused |
| **Campaign Report** | Details of **specific attack campaigns** | You were hit by / suspect a specific **attack** |
| **Threat Summary Report** | Covers **everything in both** reports above | You want the broad overview — **too wide** for a narrow actor question |

**Exam distractor:** "Threat Intelligence Report" is the **umbrella name** for the PDF opened from an alert's details pane — not one of the three types.

### What a report can contain

- Attacker identity/associations
- Attacker objectives
- Current and historical campaigns
- Attacker tactics, tools and procedures
- Indicators of compromise (IoCs) — URLs, file hashes
- Victimology — industry and geographic prevalence
- Mitigation and remediation

### Access path

Defender for Cloud → **Security alerts** → select alert → alert details → **link to report** → PDF opens (downloadable).

---

## Exam Scenario

**Scenario:** Using threat reports in Defender for Cloud. Suspect an **APT** is targeting you. Need a report that **only** covers this APT's TTPs.

| Option | Verdict | Why |
| --- | --- | --- |
| **Activity Group Report** | ✅ Correct | Group-level TTPs — narrow and actor-specific |
| Campaign Report | ❌ | One attack campaign, not the group behind it |
| Threat Summary Report | ❌ | Includes activity group **and** campaign content — broader than required |
| Threat Intelligence Report | ❌ | Umbrella name for the PDF from an individual alert, not a report type |

Keyword map: **group / actor / APT / TTPs → Activity Group** · **specific attack / campaign → Campaign** · **everything / overview → Threat Summary**.

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Activity Group vs. Campaign Report** | Who attacks you and how (actor TTPs) vs. what happened in one attack campaign. |
| **Campaign vs. Threat Summary Report** | One campaign vs. a superset of both report types. "Only" / "specific" in the question → not the summary. |
| **Defender for Cloud TI reports vs. Defender XDR Threat Analytics** | Per-alert PDF context for Azure workload alerts vs. analyst reports on active campaigns showing **your org's exposure** and related incidents across XDR. |
| **TI reports vs. Sentinel threat intelligence** | Read-only context for a human investigator vs. machine-consumable STIX indicators feeding analytics rules and hunting. See [[Threat Intelligence]]. |
| **TTPs vs. IoCs** | TTPs = behavior (hard for an attacker to change; used for attribution). IoCs = artifacts (hashes, URLs, IPs — easy to rotate). See [[Attack Chain Models]] for MITRE ATT&CK. |

---

## Exam Tips

- APT / threat actor / "group" / TTPs → **Activity Group Report**.
- "Only" or "specifically" rules out the **Threat Summary Report**.
- Reports are reached **from a security alert**, not from a standalone TI portal.
- Report detail **varies** by malware activity and prevalence.

---

## AZ-500 overlap

- AZ-500: triaging Defender for Cloud security alerts, alert details, remediation steps, workflow automation.
- Already known: where alerts live and how to respond to one.

---

## What's new in SC-100

- Using threat context (TTPs, victimology, objectives) to shape **resilience strategy**, not just close an alert.
- Picking the report whose scope matches the question — actor vs. campaign vs. everything.
- Placing Defender for Cloud reports alongside Threat Analytics and Sentinel TI in the overall [[Security Operations]] intelligence design.

---

## Keywords

- Threat intelligence report, security alert details pane, PDF
- Activity Group Report, APT, threat actor, TTPs
- Campaign Report, specific attack campaign
- Threat Summary Report, covers both
- Victimology, attacker objectives, IoCs, mitigation

---

## Related Services

- [[Microsoft Defender for Cloud]]
- [[Threat Intelligence]]
- [[Microsoft Defender XDR]]
- [[Microsoft Sentinel]]
- [[Security Operations]]
- [[Attack Chain Models]]
- [[Ransomware Resiliency and BCDR]]
- [[Microsoft Incident Response (DART)]]

---

## References

- [Threat intelligence report - Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/threat-intelligence-reports) — Microsoft Learn (read 2026-10-06)
- [[Exam Objectives]]
