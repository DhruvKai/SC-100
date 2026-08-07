---
tags:
  - sc100
type: cheat-sheet
domain:
  - ops-identity-compliance
---

# Azure Logic Apps

Serverless workflow engine — triggers and connectors chain together to automate tasks without custom code. The automation layer behind [[Microsoft Sentinel]] SOAR playbooks.

## Core Capabilities

- **Triggers** — start a run (schedule, HTTP request, event, or a Sentinel incident)
- **Connectors** — 1,000+ prebuilt actions (Azure services, M365, third-party, on-prem via data gateway)
- **Consumption plan** — multi-tenant, pay-per-execution, no VNet integration
- **Standard plan** — single-tenant, VNet integration, private endpoints, predictable pricing
- **Managed identity** — authenticate to other Azure resources without stored credentials

## Key Facts

- In a security context, a Logic App triggered by a Sentinel incident *is* a playbook — same underlying service, different name in that context.
- Standard plan is the one that supports VNet integration/private endpoints — required if the workflow needs to reach resources without public exposure.
- Secure inputs/outputs can be configured per-action to keep sensitive data out of run history logs.
- Should authenticate outbound calls using a managed identity, not embedded credentials or connection strings.

## Exam Notes

- When a scenario needs to automate a response to a detection (isolate a VM, disable a user, notify a team), the expected building block is a **Sentinel playbook**, which is a Logic App under the hood — don't overlook Logic Apps as the literal mechanism.
- Ties into "design a solution for workload identities to authenticate and access Azure resources" — Logic Apps' managed identity is a concrete example of that pattern.
- If a design requires the workflow to reach a private VNet resource, Standard plan is the correct recommendation, not Consumption.
- SOAR/workspace-topology architecture decisions live in [[Security Operations]]; this page covers the automation mechanism itself.

## Related

- [[Microsoft Sentinel]]
- [[Security Operations]]
- [[Zero Trust]]
- [[Exam Objectives]]

## References

- [Logic Apps overview](https://learn.microsoft.com/en-us/azure/logic-apps/logic-apps-overview) — Microsoft Learn
