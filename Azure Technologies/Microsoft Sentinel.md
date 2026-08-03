---
tags:
  - sc100
  - cheat-sheet
---

# Microsoft Sentinel

Cloud-native SIEM + SOAR. Ingests logs into a Log Analytics/Data Lake workspace, correlates them into incidents, and automates response.

## Core Capabilities

- **Data connectors** — ingest logs from Azure, M365, on-prem, other clouds, third-party
- **Analytics rules** — scheduled, near-real-time (NRT), and ML-based detections → generate incidents
- **Incidents** — grouped alerts with investigation graph
- **Hunting** — proactive KQL queries + notebooks over raw log data
- **Workbooks** — dashboards/visualizations
- **Playbooks** — [[Logic Apps]]-based automated response (SOAR)
- **UEBA** — behavioral baselining and anomaly detection
- **Threat intelligence** — indicator ingestion (TAXII, upload, connectors) feeding analytics rules; architecture-level detail in [[Threat Intelligence]]

## Key Facts

- Built on a Log Analytics workspace (or Microsoft Sentinel data lake tier for long-term/low-cost retention)
- Ingestion priced per GB (or via Microsoft Defender/E5 entitlements)
- Retention and hunting can span both the "analytics" tier (hot, query-ready) and the data lake tier (cheaper, long-term)
- Incidents can be created by Sentinel's own analytics rules, or ingested from [[Microsoft Defender XDR]]
- **Portal**: Sentinel is now managed through the **Microsoft Defender portal** as part of the unified security operations platform, not the Azure portal. Since July 2026 the Azure portal experience redirects automatically; it fully retires March 31, 2027.

## Exam Notes

- Sentinel = SIEM/SOAR layer; [[Microsoft Defender XDR]] = unified XDR portal correlating endpoint/identity/email/cloud-app signals — they integrate, Sentinel does not replace XDR-level correlation.
- The **unified security operations platform** (Sentinel + Defender XDR + Security Exposure Management, all in the Defender portal) is the current architecture to recommend — don't design around the legacy separate-portals model.
- MITRE ATT&CK coverage mapping is done via Sentinel's analytics rule gallery — know this maps to the exam's "evaluate threat detection coverage" objective.
- SOAR = Sentinel (detection + orchestration); XDR = Defender suite (endpoint-level detection + native response) — a scenario asking to "automate a multi-system response" points to Sentinel playbooks.

## Related

- [[Microsoft Defender XDR]]
- [[Microsoft Defender for Cloud]]
- [[Zero Trust]]
- [[Security Operations]]
- [[Threat Intelligence]]
- [[Exam Objectives]]
