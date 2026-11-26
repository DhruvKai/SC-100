---
tags:
  - sc100
type: service
domain:
  - apps-data
  - infrastructure
aliases:
  - Functions
  - Function App
  - Serverless
status: needs-verification
---

# Azure Functions

## Purpose

Serverless, event-driven compute. You deploy **code** (functions) triggered by HTTP, queues, timers or events, and Microsoft runs and patches everything underneath.

---

## Why Architects Choose It

- **Shifts OS/runtime patching to Microsoft** ([[Shared Responsibility Model]]). After a breach through an unpatched OS (e.g. a known Linux vulnerability), moving code to serverless **removes the VM attack surface** the customer was failing to patch.
- **Pay per execution** on the Consumption / Flex Consumption plans, so idle APIs cost almost nothing.
- **Automatic scale** from triggers, with no capacity planning.
- Fits **small, discrete APIs and event handlers** (payment webhook, notification sender), not whole websites.

---

## Hosting Plans

| Plan | Billing | Scale | Network isolation | Notes |
| --- | --- | --- | --- | --- |
| **Flex Consumption** | Per execution (+ optional always-ready) | Event-driven, fast | **VNet integration**, private endpoints | Microsoft's recommended serverless plan |
| **Consumption** (classic) | **Per execution only**, scales to zero | Event-driven | Limited (no VNet integration) | Linux Consumption is being retired in favour of Flex |
| **Premium** (Elastic Premium) | Always-on pre-warmed instances (**pay even when idle**) | Event-driven, no cold start | VNet integration, private endpoints | Supports **custom containers** |
| **Dedicated** (App Service plan) | Fixed plan cost | Manual/autoscale | As App Service | Reuse existing App Service capacity. Supports containers. |
| **Azure Container Apps** | Container Apps billing | KEDA-based | Container Apps environment | Functions packaged as **containers** |

> "Only pays when code is executing" is true for **Consumption/Flex Consumption**, which the exam assumes by default for "serverless Functions". It's **not** true for Premium or Dedicated.

---

## Security Controls

| Control | What it does |
| --- | --- |
| **Managed identity** | The function authenticates to Key Vault, Storage, SQL etc. with no stored credential ([[Identity and Access Management (IAM)]]) |
| **Key Vault references** | App settings point to secrets in [[Key Vault]] (`@Microsoft.KeyVault(...)`). **Opt-in, not the default.** |
| **Default secret storage** | Function/host **keys** live in the app's **Azure Storage account** (encrypted). App settings are encrypted at rest by the platform. **Not HSM-backed by default.** |
| **HTTP auth levels** | `anonymous` / `function` / `admin` keys. Keys are **shared secrets**, so put **App Service Authentication (Easy Auth / Entra ID)** or [[API Management and Security\|API Management]] in front for real auth. |
| **Network** | Access restrictions, **private endpoints**, VNet integration (Flex, Premium, Dedicated) |
| **Edge protection** | [[Front Door and Application Gateway\|Front Door]] + [[Azure Web Application Firewall\|WAF]] for internet-facing APIs |
| **Threat detection** | Defender for App Service covers Functions on App Service–based plans |

---

## When to Use

- Small, event-driven APIs and background jobs (payments, notifications, webhooks, file processing).
- Removing OS/VM patching responsibility after a VM-level compromise.
- Spiky or low-volume workloads where pay-per-execution is cheaper.

## When NOT to Use

- **Full websites / long-running web apps** → [[Azure App Service]].
- Workloads needing **OS control, custom agents** → VMs / VM Scale Sets.
- Long-running or stateful orchestration beyond Durable Functions' model → containers/AKS.
- Strict always-on low latency on Consumption (cold starts) → Premium/Flex always-ready, or App Service.

---

## Exam Scenario (got wrong): Impact of moving payment APIs to Functions

> Payment-processing web workloads on Azure were breached through a **well-known Linux vulnerability**. Recommendation: keep using Azure but move payment and notification **APIs to Azure Functions**. Describe the impact (Yes/No).

| Statement | Answer | Why |
| --- | --- | --- |
| The company is not required to manage infrastructure for the APIs | **Yes** ✅ | Serverless PaaS. Microsoft patches the OS and runtime, which fixes the root cause (an unpatched Linux VM). |
| The company's app secrets are stored in highly secure HSMs | **No** (I said Yes) | By default, keys and app settings are stored **encrypted in Azure Storage / the platform**, **not in HSMs**. HSM-backed storage only happens if *you* add **Key Vault references** to a **Premium Key Vault or Managed HSM**. Moving to Functions doesn't do that. |
| The company can deploy API workloads using hardened containers | **No** (I said Yes) | The design is **code-only serverless**: you deploy functions, not containers. Container hosting exists (Premium/Dedicated custom containers, Container Apps), but it's a different hosting choice, not an impact of "migrate to Functions", and "hardened containers" is a container-security design ([[Container and Kubernetes Security]]). |
| Web assets are stored on CDNs, not VMs | **No** ✅ | Functions runs code on the Functions platform. Nothing moves static assets to a CDN; that's a separate [[Azure CDN]] / Front Door decision. |
| The company only pays for resources when code is executing | **Yes** ✅ | Consumption/Flex billing is **per execution**, so idle = (almost) no cost. |

**The pattern:** the Yes statements are **inherent properties of serverless** (no infrastructure, pay per execution). The No statements are **other services' features** being attributed to Functions: HSMs = [[Key Vault]], hardened containers = container platform, CDN = [[Azure CDN]]. **If a statement needs another service you'd have to add on purpose, answer No.**

---

## Comparison

| Compare | Difference |
| --- | --- |
| **Functions vs. [[Azure App Service]]** | Event-triggered code, billed per execution vs. always-on web apps/APIs, billed per plan. Functions on a Dedicated plan *runs on* App Service. |
| **Consumption/Flex vs. Premium** | Pay per execution, possible cold start vs. pre-warmed instances, always billed, VNet + containers. |
| **Functions vs. containers (Container Apps/AKS)** | Deploy code, platform-managed runtime vs. deploy images, where you own image hardening and base-image patching. |
| **Function keys vs. Entra ID auth** | Shared secret in a header/query string vs. identity-based tokens. Prefer Easy Auth/APIM + Entra ID. |
| **App settings vs. Key Vault references** | Platform-encrypted config vs. secrets in Key Vault (HSM-backed only on Premium Key Vault / Managed HSM), fetched via managed identity. |
| **Functions vs. [[Logic Apps]]** | You write **code** vs. a low-code **designer + connectors**. For "automate/remediate with **minimal development effort**", Logic Apps wins. Defender for Cloud workflow automation and Sentinel playbooks are Logic Apps, not Functions ([[Defender for Cloud Workflow Automation]]). |
| **Functions vs. [[Azure Automation]]** | Event-driven app code billed per execution vs. PowerShell/Python ops runbooks, incl. on-prem via Hybrid Runbook Worker |
| **Functions vs. [[Azure Event Hubs]]** | Compute vs. a streaming pipe. A Function is often the *consumer* of an event hub (Event Hubs trigger). |

---

## AZ-500 Overlap

AZ-500 covers managed identities, Key Vault references, App Service/Functions authentication and network access restrictions. Assume the configuration.

## What's New in SC-100

- Choosing **serverless to shift patching responsibility** after an OS-level breach, as an architecture decision.
- Knowing which security properties come **with** the platform and which you must **add** (Key Vault/HSM, WAF, APIM, private endpoints).
- Matching hosting plan to cost, network isolation and cold-start requirements.

---

## Exam Tips

- "No infrastructure to manage" + "pay only when code runs" → **Functions (Consumption/Flex)**.
- "Secrets in HSMs" is **never automatic**. It needs Key Vault (Premium) or Managed HSM.
- "Hardened containers" → a container platform, not plain Functions.
- "Websites + REST APIs, autoscale, DevOps, minimal staff" → [[Azure App Service]], not Functions.
- "Evaluate and remediate Defender for Cloud alerts with workflow automation, minimize development effort" → **Logic Apps**, not Functions. Functions means writing and maintaining code.

## Common Exam Confusion

- **Serverless ≠ secretless**: you still have function keys and connection strings. Use managed identity + Key Vault.
- **Serverless ≠ always cheapest**: Premium/Dedicated bill when idle.
- **Functions ≠ CDN**: compute vs. content delivery.

---

## Keywords

- Serverless, event-driven, triggers and bindings
- Consumption, Flex Consumption, Premium (Elastic Premium), Dedicated plan
- Pay per execution, scale to zero, cold start, always-ready
- Function keys, host keys, authorization level
- Managed identity, Key Vault references
- Durable Functions

---

## Related Services

- [[Azure App Service]]
- [[Key Vault]]
- [[API Management and Security]]
- [[Shared Responsibility Model]]
- [[Container and Kubernetes Security]]
- [[Azure CDN]]
- [[Front Door and Application Gateway]]
- [[Security Updates and Patch Management]]: serverless removes OS patching
- [[Identity and Access Management (IAM)]]
- [[Logic Apps]]: low-code alternative for automation
- [[Defender for Cloud Workflow Automation]]
- [[Azure Automation]]
- [[Azure Event Hubs]]
- [[Azure Monitor Alerts and Action Groups]]: the Azure Function action type

---

## References

- [Azure Functions overview](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview) — Microsoft Learn
- [Azure Functions hosting options](https://learn.microsoft.com/en-us/azure/azure-functions/functions-scale) — Microsoft Learn
- [Securing Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/security-concepts) — Microsoft Learn
- [Use Key Vault references](https://learn.microsoft.com/en-us/azure/app-service/app-service-key-vault-references) — Microsoft Learn

---

## Verification Flag

- Hosting-plan details are from training knowledge: Flex Consumption as the recommended plan, the Linux Consumption retirement, and which plans support VNet integration and containers. Re-check them on the hosting options page.
- Default key storage (Azure Storage account blob, via `AzureWebJobsSecretStorageType`) is from training knowledge. Re-check it on the Functions security concepts page.
