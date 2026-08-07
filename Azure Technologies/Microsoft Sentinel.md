---
tags:
  - sc100
type: cheat-sheet
domain:
  - ops-identity-compliance
aliases:
  - Sentinel
status: needs-verification
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

## Convergence: Sentinel and Defender

Sentinel and [[Microsoft Defender XDR]] are no longer two products an architect *integrates* — they're delivered as one product surface, and the direction of travel is Sentinel's SIEM/data-platform capability being absorbed into Microsoft Defender rather than the two staying permanently separate-but-connected:

- **One portal, one incident queue** — not a Sentinel console feeding data into a Defender console; both surfaces render from the same Defender portal, and (with a single Primary workspace, see [[Security Operations]]) share one correlated incident list.
- **One licensing motion** — Sentinel ingestion can be covered by Microsoft Defender/E5 entitlements rather than purchased as a fully separate SIEM product.
- **New capability investment ships Defender-portal-first** — Security Copilot integration, Security Exposure Management, and unified RBAC are being built as one platform's features, not bolted onto Sentinel and Defender XDR independently.
- **What this doesn't mean**: Sentinel's SIEM capability (broad-source ingestion, long-term retention, custom analytics/hunting over raw logs) isn't disappearing — it's the reason Sentinel still exists *inside* the unified platform even as XDR's native, deep signal-source detection stays a distinct capability layered under the same product. The architecture reason to add Sentinel is unchanged (see [[Security Operations]]); what changed is that it's no longer a separate product to stand up and connect.

## Exam Notes

- Sentinel = SIEM/SOAR layer; [[Microsoft Defender XDR]] = unified XDR portal correlating endpoint/identity/email/cloud-app signals — they integrate as one product now, Sentinel does not replace XDR-level correlation, and XDR does not replace Sentinel's broad-source ingestion/retention.
- The **unified security operations platform** (Sentinel + Defender XDR + Security Exposure Management, all in the Defender portal) is the current architecture to recommend — don't design around the legacy separate-portals model, and don't describe Sentinel and Defender as separate products requiring a connector to work together.
- MITRE ATT&CK coverage mapping is done via Sentinel's analytics rule gallery — know this maps to the exam's "evaluate threat detection coverage" objective.
- SOAR = Sentinel (detection + orchestration); XDR = Defender suite (endpoint-level detection + native response) — a scenario asking to "automate a multi-system response" points to Sentinel playbooks.

## Verification Flag

The Sentinel/Defender convergence is an active, ongoing process, not a completed one-time rename — Microsoft continues to shift capability and packaging between the two names. Re-verify current portal behavior, licensing bundling, and exactly which capabilities are Sentinel-branded vs. Defender-branded close to exam date.

## Related

- [[Microsoft Defender XDR]]
- [[Microsoft Defender for Cloud]]
- [[Zero Trust]]
- [[Security Operations]]
- [[Threat Intelligence]]
- [[Exam Objectives]]
