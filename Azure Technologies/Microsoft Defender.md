---
tags:
  - sc100
  - cheat-sheet
---

# Microsoft Defender (Family Overview)

Umbrella brand for Microsoft's threat protection products. [[Microsoft Defender XDR]] is the unifying portal that correlates signals across the workload-specific products below into single incidents.

## Product Map

| Product | Protects | One-line purpose |
| --- | --- | --- |
| [[Microsoft Defender XDR]] | Cross-workload | Unified incident correlation across endpoint, identity, email, and cloud apps |
| [[Microsoft Defender for Cloud]] | Cloud resources | CSPM (posture/Secure Score) + CWPP (workload protection) across hybrid/multicloud |
| Defender for Endpoint | Devices | EDR — endpoint detection & response |
| Defender for Office 365 | Email/collaboration | Phishing, malware, Safe Links/Attachments |
| Defender for Identity | On-prem AD DS | Sensor-based detection on domain controllers |
| Defender for Cloud Apps | SaaS apps | CASB — SaaS discovery, control, session policies |
| Defender for IoT | OT/ICS/IoT | Device discovery and threat detection for operational tech |
| Defender for Storage | Blob storage | Malware scanning, anomalous access detection |
| Defender for Databases | SQL, Cosmos DB, etc. | Threat detection for database workloads |
| Defender for Containers | Containers | Image scanning + runtime protection (a Defender for Cloud plan) |
| Defender EASM | Internet-facing assets | External attack surface discovery/monitoring |

## Key Facts

- Defender for Cloud is the CSPM/CWPP layer; the workload-specific plans within it (Containers, Databases, Storage, etc.) are enabled per-resource-type.
- Defender XDR does not replace the individual products — it's the correlation/incident layer sitting above them.
- Most Defender products flow their alerts into [[Microsoft Sentinel]] for SIEM-level correlation with everything else.

## Exam Notes

- Don't confuse **Defender for Cloud** (posture + cloud workload protection) with **Defender XDR** (endpoint/identity/email/app correlation) — a frequent exam distractor pair.
- **Defender for Cloud vs. Azure Policy**: Defender for Cloud evaluates and scores posture (incl. via MCSB); Azure Policy enforces/denies configuration — Defender for Cloud can use Azure Policy under the hood for its assessments.
- Full technical deep-dives on [[Microsoft Defender for Cloud]] belong in its own note under `03 Infrastructure` — this page is an orientation map only.

## Related

- [[Microsoft Sentinel]]
- [[Microsoft Defender for Cloud]]
- [[Microsoft Defender XDR]]
- [[Zero Trust]]
- [[Exam Objectives]]
