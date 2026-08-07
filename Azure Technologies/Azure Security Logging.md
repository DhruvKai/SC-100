---
tags:
  - sc100
type: cheat-sheet
domain:
  - ops-identity-compliance
status: needs-verification
---

# Azure Security Logging

Six core log sources architects combine into a centralized logging design.

## Log Sources

| Source                                | Plane          | What it captures                                                                                    |
| ------------------------------------- | -------------- | --------------------------------------------------------------------------------------------------- |
| Activity Log                          | Control plane  | Subscription-level record of every ARM Azure Resource Manager operation (who did what, when)        |
| Azure Resource Logs (Diagnostic logs) | Data plane     | Operations performed _within_ a resource (e.g. queries against a database, requests to a Key Vault) |
| Virtual Machines                      | Data plane     | Guest OS logs (Windows Event Log, Linux syslog) via Azure Monitor Agent                             |
| Azure Storage Analytics               | Data plane     | Insights on requests made to storage accounts                                                       |
| Network flow logs                     | Data plane     | Inbound/outbound traffic via network security groups or virtual networks                            |
| Microsoft Entra ID reports            | Identity plane | Sign-in activity, audit logs, and other directory activity                                          |

## Key Facts

- **Activity Log vs. Resource Log** is the core distinction: Activity Log = "was this resource created/modified/deleted" (control plane); Resource Log = "what happened inside the resource" (data plane). Both route to a Log Analytics workspace, Storage, or Event Hub via diagnostic settings.
- **Network flow logs**: NSG flow logs are retiring — new NSG flow logs can no longer be created (cutoff passed June 30, 2025), and existing ones retire September 30, 2027. **Virtual Network (VNet) flow logs** are the current recommended source; design new solutions around VNet flow logs, not NSG flow logs. Mechanics of the [[Network Watcher and Lighthouse|Network Watcher]] tooling underneath flow logs/Traffic Analytics live in [[Network Watcher and Lighthouse]], not repeated here.
- **Microsoft Entra ID reports** cover both sign-in logs (authentication events) and audit logs (directory changes — e.g., role assignments, app registrations).
- VM guest logs require the Azure Monitor Agent (AMA); Arc-enabled servers extend the same collection to hybrid/on-prem and multicloud VMs.

## Diagnostic Settings

The resource-level configuration object that actually routes platform logs/metrics to a destination — nothing above reaches Sentinel or long-term storage without one.

- **Destinations** — a Log Analytics workspace (for Sentinel/KQL analysis), a Storage account (cheap long-term/compliance retention), an Event Hub (streaming out to a SIEM or partner tool), or a partner integration — one diagnostic setting can target **multiple destinations simultaneously**.
- **Up to 5 diagnostic settings per resource** — lets the same log data go to Sentinel *and* cold storage at once, a common compliance pattern (hot data for detection, cold data for retention/audit).
- **Category groups** — `audit` (security/compliance-relevant categories only) vs. `allLogs` (everything the resource emits) — simplifies category selection instead of enumerating every log category by hand.
- **Not retroactive** — a diagnostic setting only captures logs from the point it's created forward; turning it on after an incident doesn't backfill what already happened.
- **Enforced at scale via [[Azure Policy]]** (`DeployIfNotExists` effect) — the same governance-at-scale pattern used everywhere else in this vault; manually configuring diagnostic settings resource-by-resource doesn't scale to a portfolio.

## Azure Monitor Agent (AMA) and Data Collection Rules (DCR)

- **AMA is the single, unified successor** to the legacy Log Analytics Agent (MMA/OMS agent) and the Diagnostics extension — both are **retired** (Log Analytics Agent retirement completed August 31, 2024). Any current design should name AMA, not a legacy agent.
- **A Data Collection Rule (DCR) is what tells AMA *what* to collect and *where to send it*** — which Windows Event Log channels, which Syslog facilities, which performance counters, routed to which Log Analytics workspace/table. The agent alone collects nothing without an associated DCR.
- **Multi-homing** — one VM can have multiple DCRs associated for different collection purposes (e.g., one DCR feeding Sentinel's security events table, a separate DCR feeding a performance-monitoring workspace).
- **Does Defender for Cloud need it?** Defender for Servers Plan 2 **auto-provisions** AMA plus a Defender-managed DCR scoped to its own security telemetry needs — largely automatic, not something an architect hand-builds. See [[Cloud Workload Protection (CWPP)]] for the plan itself.
- **Does Sentinel need it?** Yes, explicitly, and it's *not* automatic — Sentinel's Windows Security Events, Syslog/CEF, and most current data connectors are DCR-based, replacing the legacy agent-based connector type. Onboarding these sources requires the architect to actively define/associate the DCR (which event IDs, which facilities) — this is the real contrast: Defender for Cloud mostly manages its own DCR for you, Sentinel data connectors require you to configure it. See [[Microsoft Sentinel]].

## Exam Notes

- SC-100 objective explicitly calls out designing **centralized logging and auditing, including Microsoft Purview Audit** — these six sources are the raw inputs; the architecture decision is where they converge (typically a Log Analytics workspace feeding [[Microsoft Sentinel]]).
- "Design monitoring to support hybrid and multicloud environments" → Azure Monitor Agent + [[Azure Arc]] is the expected answer for non-Azure/on-prem VM log collection.
- A scenario needing traffic-flow visibility should point to VNet flow logs, not classic NSG flow logs — watch for exam content still referencing the older name.
- These sources are the connectors that feed [[Microsoft Sentinel]] analytics rules and [[Microsoft Defender for Cloud]] posture assessments — the AZ-500-level knowledge is _where each log comes from_; the SC-100 addition is _designing which sources centralize where_ for hybrid/multicloud coverage.
- "Enable logging across a large resource portfolio without manual per-resource configuration" → diagnostic settings deployed via [[Azure Policy]] (`DeployIfNotExists`), not a manual rollout.
- "Onboard Windows Security Events to Sentinel" → requires defining/associating a DCR explicitly; don't assume it's automatic the way Defender for Servers Plan 2's own AMA provisioning is.
- A scenario naming the legacy **Log Analytics Agent (MMA)** or "Diagnostics extension" is describing a retired mechanism — the current answer is always AMA + DCR.

## Comparison

| Compare | Difference |
| --- | --- |
| Legacy Log Analytics Agent (MMA) vs. Azure Monitor Agent (AMA) | MMA/OMS agent and the Diagnostics extension are **retired** (August 31, 2024). AMA is the single current agent for all VM guest log/metric collection, Azure and Arc-enabled alike — any current design names AMA, never MMA. |
| Defender for Servers' DCR vs. Sentinel's DCR | Defender for Servers Plan 2 auto-provisions its own AMA + DCR for security telemetry — largely hands-off. Sentinel's DCR-based connectors (Windows Security Events, Syslog/CEF) require the architect to explicitly define and associate the DCR — not automatic. Same underlying mechanism (AMA + DCR), different operational burden. |
| Diagnostic settings vs. Azure Policy-enforced diagnostic settings | A diagnostic setting configured manually applies to one resource. Enforcing diagnostic settings via [[Azure Policy]] (`DeployIfNotExists`) applies the same configuration automatically to every matching resource in scope, including new ones as they're created — the portfolio-scale answer. |

## Related

- [[Microsoft Sentinel]]
- [[Microsoft Defender for Cloud]]
- [[Security Operations]]
- [[Cloud Workload Protection (CWPP)]]
- [[Azure Arc]]
- [[Azure Policy]]
- [[Network Watcher and Lighthouse]]
- [[Exam Objectives]]

## References

- [Virtual Network Flow Logs overview](https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-overview) — Microsoft Learn
- [Migrate to Virtual Network Flow Logs](https://learn.microsoft.com/en-us/azure/network-watcher/nsg-flow-logs-migrate) — Microsoft Learn
- [Azure Monitor Agent overview](https://learn.microsoft.com/en-us/azure/azure-monitor/agents/agents-overview) — Microsoft Learn
- [Data collection rules (DCR) overview](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-collection-rule-overview) — Microsoft Learn
- [Diagnostic settings overview](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/diagnostic-settings) — Microsoft Learn

## Verification Flag

AMA/DCR is an actively-evolving area — the exact set of Sentinel connectors still on legacy agent-based collection (vs. fully migrated to DCR-based) shifts over time, and Microsoft has periodically extended/changed agent retirement dates. Re-verify current connector status and dates against the live Azure Monitor Agent/DCR overview pages close to exam date.
