---
tags:
  - sc100
  - cheat-sheet
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
- **Network flow logs**: NSG flow logs are retiring — new NSG flow logs can no longer be created (cutoff passed June 30, 2025), and existing ones retire September 30, 2027. **Virtual Network (VNet) flow logs** are the current recommended source; design new solutions around VNet flow logs, not NSG flow logs.
- **Microsoft Entra ID reports** cover both sign-in logs (authentication events) and audit logs (directory changes — e.g., role assignments, app registrations).
- VM guest logs require the Azure Monitor Agent (AMA); Arc-enabled servers extend the same collection to hybrid/on-prem and multicloud VMs.

## Exam Notes

- SC-100 objective explicitly calls out designing **centralized logging and auditing, including Microsoft Purview Audit** — these six sources are the raw inputs; the architecture decision is where they converge (typically a Log Analytics workspace feeding [[Microsoft Sentinel]]).
- "Design monitoring to support hybrid and multicloud environments" → Azure Monitor Agent + Azure Arc is the expected answer for non-Azure/on-prem VM log collection.
- A scenario needing traffic-flow visibility should point to VNet flow logs, not classic NSG flow logs — watch for exam content still referencing the older name.
- These sources are the connectors that feed [[Microsoft Sentinel]] analytics rules and [[Microsoft Defender for Cloud]] posture assessments — the AZ-500-level knowledge is _where each log comes from_; the SC-100 addition is _designing which sources centralize where_ for hybrid/multicloud coverage.

## Related

- [[Microsoft Sentinel]]
- [[Microsoft Defender for Cloud]]
- [[Security Operations]]
- [[Exam Objectives]]

## References

- [Virtual Network Flow Logs overview](https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-overview) — Microsoft Learn
- [Migrate to Virtual Network Flow Logs](https://learn.microsoft.com/en-us/azure/network-watcher/nsg-flow-logs-migrate) — Microsoft Learn
