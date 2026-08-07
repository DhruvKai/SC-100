---
tags:
  - sc100
type: cheat-sheet
domain:
  - infrastructure
  - ops-identity-compliance
status: needs-verification
---

# Network Watcher and Lighthouse

Two unrelated services bundled on one page for quick reference — **Network Watcher** is regional network diagnostics/monitoring; **Azure Lighthouse** is cross-*tenant* delegated administration. The only thing they share is being background infrastructure most architectures need but rarely feature as the main decision.

## Azure Network Watcher

Auto-enabled per region the first time a VNet is created there (a `NetworkWatcher_<region>` resource group appears) — a regional diagnostics toolkit, not a resource you deploy from scratch.

- **VNet/NSG flow logs** — the actual log source; mechanics and the NSG→VNet flow log migration already covered in [[Azure Security Logging]], not repeated here. **Traffic Analytics** is the analytics layer built on top of flow logs, visualizing traffic patterns and surfacing anomalies.
- **IP flow verify** — tests whether a specific packet (source/destination IP/port, protocol) would be allowed or denied by the NSG rules in effect, without generating real traffic — the fast answer to "is this NSG rule actually blocking my traffic."
- **Next hop** — shows what the effective next hop is for traffic from a VM toward a given destination (useful for diagnosing an unexpected route table entry).
- **Connection troubleshoot / Connection monitor** — one-time connectivity test vs. continuous, scheduled monitoring between a source and destination (VM, FQDN, URI) — troubleshoot is point-in-time, monitor is ongoing.
- **Packet capture** — on-demand packet-level capture on a VM, triggerable manually or by an alert, for deep protocol-level troubleshooting.
- **Topology** — visualizes the resources and relationships within a VNet.

### Exam Notes

- "Verify whether a specific NSG rule is actually blocking a given flow" → IP flow verify, not a manual rule-by-rule read-through.
- "Continuously monitor connectivity between two resources and alert on failure" → Connection monitor, not one-off Connection troubleshoot.
- "Visualize traffic patterns and anomalies from flow logs" → Traffic Analytics (built on VNet flow logs, which are covered in [[Azure Security Logging]]).

---

## Azure Lighthouse

Delegates administration of resources in a **customer's** Azure tenant to principals in a **different, managing** tenant — without guest (B2B) accounts in every customer tenant and without the managing team switching directories.

- **Delegation** is defined via an ARM template (or a Marketplace managed-service offer) that specifies which Azure roles are granted to which principals from the managing tenant, scoped to specific subscriptions or resource groups in the customer tenant.
- The customer retains full visibility and control — delegated actions show up in the customer's own Activity Log, and the customer can revoke the delegation at any time.
- Typical use: MSPs/MSSPs managing many customer tenants centrally, or a large enterprise with a central platform team managing resources across separately-governed subsidiary tenants.
- **Not** the mechanism for projecting non-Azure resources into Azure management — that's [[Azure Arc]]. Full Arc-vs-Lighthouse comparison lives in [[Azure Arc]], not repeated here.

### Exam Notes

- "An MSP needs to manage many customers' Azure resources centrally, without guest accounts in each tenant" → Azure Lighthouse.
- "Bring a non-Azure server/Kubernetes cluster/SQL Server under Azure management" → [[Azure Arc]], not Lighthouse — different problem (resources outside Azure vs. resources in someone else's Azure tenant).
- The customer can always see and revoke what the managing tenant has done — a scenario implying loss of customer visibility/control is describing Lighthouse incorrectly.

---

## Comparison

| Compare | Difference |
| --- | --- |
| Network Watcher vs. Traffic Analytics | Network Watcher is the diagnostics toolkit (flow logs, IP flow verify, packet capture, connection monitor). Traffic Analytics is the visualization/analytics layer built specifically on top of flow log data — a feature of Network Watcher, not a separate product. |
| Azure Lighthouse vs. Azure Arc | Full comparison in [[Azure Arc]] — Lighthouse delegates management of resources already in *another Azure tenant*; Arc projects resources that live *outside Azure entirely* into the managing tenant's own Azure Resource Manager. Different problem, easy to conflate since both extend "who can manage what." |

---

## AZ-500 Review

AZ-500 already covers using Network Watcher's individual tools (flow logs, IP flow verify, connection troubleshoot) and setting up basic Lighthouse delegation at a configuration level. SC-100 adds: recognizing which diagnostic tool a described symptom points to without being told the tool name, and treating Lighthouse as a named architecture answer for MSP/multi-tenant management scenarios rather than a niche feature.

---

## Keywords

- Network Watcher, IP flow verify, Next hop
- Connection troubleshoot vs. Connection monitor
- Packet capture, Topology
- Traffic Analytics (built on VNet flow logs)
- Azure Lighthouse, cross-tenant delegated administration
- Managing tenant vs. customer tenant

---

## Related Services

- [[Azure Security Logging]]
- [[Network Security Architecture]]
- [[Network Security Group]]
- [[Azure Arc]]
- [[Security Operations]]
- [[Exam Objectives]]

---

## References

- [Azure Network Watcher overview](https://learn.microsoft.com/en-us/azure/network-watcher/network-watcher-overview) — Microsoft Learn
- [What is Azure Lighthouse?](https://learn.microsoft.com/en-us/azure/lighthouse/overview) — Microsoft Learn

---

## Verification Flag

Network Watcher's tool list has shifted before (some capabilities have been deprecated/consolidated); re-verify the current tool set against the live overview page close to exam date.
