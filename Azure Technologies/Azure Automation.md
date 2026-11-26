---
tags:
  - sc100
type: service
domain:
  - infrastructure
  - ops-identity-compliance
aliases:
  - Automation account
  - Runbooks
  - Automation runbook
  - Hybrid Runbook Worker
status: needs-verification
---

# Azure Automation

## Purpose

Cloud script automation: **runbooks** (PowerShell/Python) run on a schedule, by webhook or from an alert, against Azure or (through a **Hybrid Runbook Worker**) on-prem and other clouds.

---

## Why Architects Choose It

- **Script-based ops tasks** admins already write in PowerShell: start/stop VMs, rotate a setting, clean up resources, run a script inside a server.
- **Hybrid reach**: Hybrid Runbook Workers run runbooks on on-prem / Arc-enabled machines, where a Logic App can't execute code.
- **Managed identity** for authentication. The old Run As accounts are retired.
- **Private networking**: the only Azure Monitor action group action type documented as supporting Private Link ([[Azure Monitor Alerts and Action Groups]]).

---

## When to Use

- Scheduled or alert-driven **scripts** (PowerShell/Python) for operations and remediation.
- Remediation that must run **inside** on-prem/hybrid servers (Hybrid Runbook Worker).
- Teams standardized on PowerShell for ops.

## When NOT to Use

- **Defender for Cloud workflow automation** → it triggers **Logic Apps only**. A Logic App can call a runbook if you really need script steps ([[Defender for Cloud Workflow Automation]]).
- **Sentinel SOAR** → playbooks (Logic Apps) ([[Playbooks and Automation Rules]]).
- **Patching** → [[Azure Update Manager]]. Azure Automation Update Management retired 31 Aug 2024.
- **Configuration drift / baselines** → Azure Machine Configuration ([[Azure Policy]] guest configuration) rather than Automation State Configuration (DSC).
- **Low-code multi-system workflows with connectors** → [[Logic Apps]].

---

## Comparison

| Compare | Difference |
| --- | --- |
| **Automation runbook vs. [[Logic Apps]]** | Script you write (PowerShell/Python) vs. visual workflow from connectors. Logic Apps = lower development effort for cross-service work. Runbooks = scripted admin tasks, hybrid execution. |
| **Automation runbook vs. [[Azure Functions]]** | Ops scripts on a schedule/webhook, long-running jobs vs. event-driven app code, per-execution billing, triggers and bindings |
| **Hybrid Runbook Worker vs. [[Azure Arc]]** | Runs runbooks on a machine vs. projects the machine into ARM. Arc is the onboarding path, and the worker can be deployed as an extension on Arc servers. |
| **Azure Automation Update Management vs. [[Azure Update Manager]]** | Retired (Automation + Log Analytics dependency) vs. current native service |

---

## AZ-500 Overlap

AZ-500 touches Automation accounts, managed identities for runbooks and the legacy Update Management. That's assumed.

## What's New in SC-100

- Picking the **right automation engine** for the requirement: Logic Apps (low-code, security triggers) vs. Functions (code) vs. Automation (scripts, hybrid).
- Recognizing Automation features that have been **replaced** (Update Management → Update Manager, Run As → managed identity, DSC → Machine Configuration).

---

## Exam Tips

- "Defender for Cloud / Sentinel automated response" → Logic Apps, **not** Automation runbooks.
- "Run a PowerShell script on on-prem servers from Azure on a schedule" → **Azure Automation + Hybrid Runbook Worker**.
- "Patch management via Azure Automation" → legacy. The current answer is **Azure Update Manager**.

## Common Exam Confusion

- **Azure Automation vs. "workflow automation"**: an Automation account runs scripts. *Workflow automation* is the Defender for Cloud feature that triggers Logic Apps.
- **Runbook vs. playbook**: a script in Azure Automation vs. a Logic App in Sentinel.

---

## Keywords

- Automation account, runbook (PowerShell, Python), schedule, webhook
- Hybrid Runbook Worker
- Managed identity (Run As accounts retired)
- State Configuration (DSC), Change Tracking and Inventory
- Update Management (retired)

---

## Related Services

- [[Logic Apps]]
- [[Azure Functions]]
- [[Azure Monitor Alerts and Action Groups]]
- [[Defender for Cloud Workflow Automation]]
- [[Azure Update Manager]]
- [[Azure Arc]]
- [[Azure Policy]]
- [[Security Updates and Patch Management]]

---

## References

- [Azure Automation overview](https://learn.microsoft.com/en-us/azure/automation/overview) — Microsoft Learn
- [Hybrid Runbook Worker overview](https://learn.microsoft.com/en-us/azure/automation/automation-hybrid-runbook-worker) — Microsoft Learn

---

## Verification Flag

- The Run As account retirement (Sept 2023) and the State Configuration (DSC) retirement / move to Machine Configuration are from training knowledge, and DSC dates have shifted before. Re-check them on the Azure Automation overview and retirement pages.
