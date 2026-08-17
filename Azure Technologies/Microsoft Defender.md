---
tags:
  - sc100
type: cheat-sheet
domain:
  - infrastructure
---
# Microsoft Defender (Family Overview)

Umbrella brand for Microsoft's threat protection products. [[Microsoft Defender XDR]] is the unifying portal that correlates signals across the workload-specific products below into single incidents.
![Defender product map overview](../Images/Pasted%20image%2020260804162451.png)

![Defender XDR signal correlation](../Images/Pasted%20image%2020260804162621.png)
## Product Map

| Product | Protects | One-line purpose |
| --- | --- | --- |
| [[Microsoft Defender XDR]] | Cross-workload | Unified incident correlation across endpoint, identity, email, and cloud apps — AIR, Attack Disruption, Advanced Hunting, Unified RBAC detailed in its own note |
| [[Microsoft Defender for Cloud]] | Cloud resources | [[Security Posture Assessments\|CSPM]] (posture/Secure Score) + [[Cloud Workload Protection (CWPP)\|CWPP]] (workload protection) across hybrid/multicloud |
| Defender for Endpoint | Devices | EDR — endpoint detection & response; endpoint architecture in [[Securing Server and Client Endpoints]] |
| Defender for Office 365 | Email/collaboration | Phishing, malware, Safe Links/Attachments |
| Defender for Identity | On-prem AD DS | Sensor-based detection on domain controllers |
| Defender for Cloud Apps | SaaS apps | CASB — SaaS discovery, control, session policies; full depth in [[SaaS Application Discovery and Control]] |
| Defender for IoT | OT/ICS/IoT | Device discovery and threat detection — Enterprise vs. OT editions in [[Securing Server and Client Endpoints]] |
| Defender for Storage | Blob storage | Malware scanning, anomalous access detection |
| Defender for Databases | SQL, Cosmos DB, etc. | Threat detection for database workloads |
| Defender for Containers | Containers | Image scanning + runtime protection (a Defender for Cloud plan) |
| Defender EASM | Internet-facing assets | External attack surface discovery/monitoring — see [[CSPM and CWPP]] for how it feeds the Cloud Security Graph |

## Key Facts

- Defender for Cloud is the CSPM/CWPP layer; the workload-specific plans within it (Containers, Databases, Storage, etc.) are enabled per-resource-type.
- Defender XDR does not replace the individual products — it's the correlation/incident layer sitting above them.
- Most Defender products flow their alerts into [[Microsoft Sentinel]] for SIEM-level correlation with everything else — and Sentinel itself now renders inside this same Defender portal rather than a separate console (see [[Microsoft Sentinel]]'s Convergence section).

## Exam Notes

- Don't confuse **Defender for Cloud** (posture + cloud workload protection) with **Defender XDR** (endpoint/identity/email/app correlation) — a frequent exam distractor pair.
- **Defender for Cloud vs. Azure Policy**: Defender for Cloud evaluates and scores posture (incl. via MCSB); Azure Policy enforces/denies configuration — Defender for Cloud can use Azure Policy under the hood for its assessments. In fact, **Azure Policy drives Secure Score**: full mechanics in [[Azure Policy]].
- Full technical deep-dives on [[Microsoft Defender for Cloud]] belong in its own note under `03 Infrastructure` — this page is an orientation map only. CSPM lives in [[Security Posture Assessments]], CWPP in [[Cloud Workload Protection (CWPP)]], and how they combine in [[CSPM and CWPP]].


(https://aka.ms/xdr)


## Related

- [[Microsoft Sentinel]]
- [[Microsoft Defender for Cloud]]
- [[Microsoft Defender XDR]]
- [[Security Posture Assessments]]
- [[Cloud Workload Protection (CWPP)]]
- [[Securing Server and Client Endpoints]]
- [[Zero Trust]]
- [[Exam Objectives]]
