---
tags:
  - sc100
type: concept
domain:
  - applications
aliases:
  - App Service
  - Azure Web Apps
  - Web App
status: needs-verification
---

# Azure App Service

## Purpose

Fully managed PaaS for hosting **web apps, REST APIs and mobile back ends**, with built-in autoscale, CI/CD integration (Azure DevOps/GitHub) and platform-managed patching.

---

## Why architects choose it

- **Minimal administrative overhead.** Microsoft patches the OS and runtime, which shifts responsibility left in the [[Shared Responsibility Model]].
- **Autoscale** on metrics or schedule without managing VMs.
- Native **Azure DevOps / GitHub Actions** deployment and **deployment slots** for safe swaps.
- Security features built in: Easy Auth ([[Entra ID]]), managed identity → [[Key Vault]], access restrictions, Private Endpoint / VNet integration (details in [[Securing IaaS and PaaS Services]]).
- Protected by Defender for App Service ([[Microsoft Defender for Cloud]]).

---

## When to use

- Hosting websites or REST APIs when the requirement says **"minimize administrative overhead"**, **"scale automatically"** or **"integrate with Azure DevOps"**.
- Migrating IaaS-hosted web workloads to PaaS to reduce the patching and scaling burden.

---

## When NOT to use

- Event-driven, short-lived code (process an image, send a notification, handle a payment event) → **Azure Functions**.
- Spring Boot microservices platform → **Azure Spring Apps** (retiring, so check status).
- Full OS control, custom agents or lift-and-shift VMs → **VM Scale Sets** (autoscale but still IaaS).
- Container orchestration at scale → AKS / Container Apps (see [[Container and Kubernetes Security]]).

---

## Compute Selection Comparison

| Service | Model | Autoscale | Admin overhead | Typical exam trigger |
| --- | --- | --- | --- | --- |
| **App Service** | PaaS web hosting | Yes | **Lowest** for sites/APIs | Websites + REST APIs, DevOps, minimal staff |
| **Azure Functions** | Serverless, event-triggered | Yes (consumption: pay per execution) | Very low | Small pieces of code triggered by events |
| **Azure Spring Apps** | Managed Spring Boot microservices | Yes | Low | Spring Boot, service discovery, config server |
| **VM Scale Sets** | IaaS pool of identical VMs | Yes | **High** (OS, patching, images) | Need VMs, custom OS, autoscale |
| **AKS** | Managed Kubernetes | Yes | Medium–high | Containers, microservices orchestration |

---

## Exam Scenario

Same scenario as [[Azure CDN]]: IaaS-hosted websites and REST APIs, minimal IT staff, a DDoS-driven outage and revenue loss.

- **Scaling + DevOps + minimal overhead → App Service.**
- **DDoS → Azure CDN.**
- Functions, Spring Apps and VMSS each satisfy part of the requirement but lose on workload fit (Functions, Spring Apps) or admin overhead (VMSS).

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **App Service vs. VMSS** | Both autoscale. App Service is PaaS (no OS management); VMSS is IaaS (you patch and maintain the VMs). "Minimal overhead" means App Service. |
| **App Service vs. Functions** | Long-running web apps/APIs vs. event-triggered snippets billed per execution. |
| **App Service vs. Spring Apps** | General web hosting vs. a Spring Boot–specific microservices platform. |
| **App Service vs. Container Apps / AKS** | Code or single container web hosting vs. container orchestration. |

---

## Exam Tips

- "Websites + REST APIs" + "minimal staff/overhead" + "autoscale" + "Azure DevOps" → **App Service**.
- An autoscale requirement alone doesn't mean VMSS. Check for **admin overhead** wording.
- Functions = **event-driven** wording ("triggered", "process", "pay only when code runs").

---

## AZ-500 overlap

- Known: App Service authentication, access restrictions, managed identities, TLS/certificates, Defender for App Service.
- New: choosing App Service over IaaS/serverless as an architecture decision driven by operations and risk.

---

## What's new in SC-100

- Mapping business constraints (staffing, revenue loss, DevOps) to compute models.
- Reducing attack surface and operational risk by moving web workloads from IaaS to PaaS.

---

## Keywords

- PaaS, web apps, REST APIs, mobile back ends
- Autoscale, deployment slots, Azure DevOps, GitHub Actions
- Minimal administrative overhead, minimal IT staff
- Azure Functions, Azure Spring Apps, Virtual Machine Scale Sets
- Easy Auth, managed identity, access restrictions

---

## Related Services

- [[Securing IaaS and PaaS Services]]
- [[Azure CDN]]
- [[Front Door and Application Gateway]]
- [[Azure Web Application Firewall]]
- [[DevOps Security]]
- [[Shared Responsibility Model]]
- [[Microsoft Defender for Cloud]]
- [[Key Vault]]
- [[Container and Kubernetes Security]]

---

## References

- [App Service overview](https://learn.microsoft.com/en-us/azure/app-service/overview) — Microsoft Learn
- [Azure Functions overview](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview) — Microsoft Learn
- [What is Azure Spring Apps?](https://learn.microsoft.com/en-us/azure/spring-apps/basic-standard/overview) — Microsoft Learn
- [Virtual Machine Scale Sets documentation](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/) — Microsoft Learn

---

## Verification Flag

- Azure Spring Apps has an announced retirement. Re-check its status and dates; the exam may still list it as a distractor.
