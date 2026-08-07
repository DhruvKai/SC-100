---
tags:
  - sc100
type: concept
domain:
  - best-practices
  - ops-identity-compliance
  - infrastructure
  - apps-data
status: needs-verification
---

# AI and Copilot Security Architecture

## Purpose

How generative AI and Copilot reshape SC-100 architecture in two directions: AI as a new workload to secure (data, apps, agents) and AI as a new capability that secures other things ([[Microsoft Security Copilot]]).

---

## Why Architects Choose It

- AI adoption is happening regardless of security readiness — [[Cloud Adoption Framework (CAF)|CAF]]'s Secure AI guidance treats this as "secure what's being adopted," not "block AI."
- [[Microsoft Security Copilot|Security Copilot]] gives defenders a productivity lift (natural-language KQL, incident summarization, guided response) embedded in tools the SOC already uses, not a new console.
- Microsoft 365 Copilot doesn't grant new access — it *inherits* the signed-in user's existing permissions, turning pre-existing oversharing into an immediately discoverable, exploitable leak. Data governance becomes a prerequisite, not hygiene.
- Autonomous AI agents need their own identity primitive — reusing a shared service principal or embedding secrets in agent code doesn't scale, audit, or support least privilege.

---

## When to Use

- Rolling out Microsoft 365 Copilot, Copilot Chat, or any enterprise generative AI tool — run a [[Data Security Posture Management (DSPM)|Purview DSPM]] data risk assessment first.
- Equipping a SOC with faster triage and investigation — Security Copilot embedded in [[Microsoft Defender XDR]] / [[Microsoft Sentinel]] / [[Entra ID]] / Intune / Purview.
- Building or hosting custom generative AI apps on Azure AI Foundry — [[Microsoft Defender for Cloud]] AI security posture management (AI-SPM) and AI threat protection.
- Any AI agent that authenticates, calls tools, or acts autonomously — provision it through [[Microsoft Entra Agent ID]], not a shared app registration.

---

## When NOT to Use

- Treating Microsoft 365 Copilot as low-risk because "it's just a chat interface" — it surfaces any content the signed-in user can already reach.
- Standing up Security Copilot without planning Security Compute Unit (SCU) capacity and workspace RBAC — uncontrolled cost and access sprawl follow.
- Managing agent identities by hand through app registrations once agent counts grow — that's the scaling problem Agent ID blueprints solve.

---

## Data Security Posture Management (DSPM) for AI

- Within [[Data Security Posture Management (DSPM)|Purview DSPM]], AI risk lives in a dedicated **AI observability** page — an inventory of AI apps and agents (including Microsoft Agent 365) active in the last 30 days, with high-risk counts and sensitive-interaction breakdowns.
- **Data Risk Assessments** (under Discover) are the standard pre-Copilot-rollout check — they scan for oversharing (e.g., an over-permissioned SharePoint site) that Copilot would otherwise surface to any user who can already reach it.
- **Activity explorer → AI activities** tracks individual genAI prompts/responses for sensitive-information matches and DLP rule hits, feeding investigation and compliance workflows.
- "DSPM for AI" as a standalone product is now the **classic**, deprecated predecessor — current guidance folds AI risk into the unified Purview DSPM experience above.

For DSPM's general, non-AI data-estate scope — and its comparison to Defender for Cloud's data-aware security posture — see [[Data Security Posture Management (DSPM)]].

---

## Azure AI Services Security

Securing the AI service itself — Azure OpenAI, Azure AI Foundry, and the broader Azure AI services family — as a PaaS workload, following the same network/identity/encryption pattern used everywhere else in this vault, plus AI-specific content filtering that has no equivalent on a normal PaaS resource.

- **Content Safety / Prompt Shields** — Azure AI Content Safety filters harmful content (hate, violence, sexual, self-harm) in both prompts and model output. **Prompt Shields** specifically detects and blocks **jailbreak attempts** (a user directly trying to override system instructions) and **indirect prompt injection** (malicious instructions hidden in third-party content the model processes — a document, email, or web page it's asked to summarize) — the concrete, in-line *preventive* control for the "prompt injection" threat category, distinct from Defender for Cloud's out-of-band *detective* layer (see Comparison). Full AI-specific threat taxonomy (prompt injection, excessive agency, etc.) is in [[Threat Modeling]].
- **Network isolation** — disable public network access on Azure OpenAI/AI Foundry resources by default, then use **Private Link** (same pattern as [[Securing IaaS and PaaS Services]]); Azure AI Foundry hubs additionally support a **managed virtual network** scoping outbound access from the compute the hub provisions.
- **Customer-managed keys (CMK)** — encrypt fine-tuned models and stored training data with a customer-managed key in [[Key Vault]] instead of the Microsoft-managed default, the same CMK decision already covered in [[Data Classification and Protection]].
- **RBAC for AI Foundry** — hub/project-scoped Azure roles (e.g., a role that can deploy/manage a model vs. a narrower role that can only call it) separate *who can change the model* from *who can only use it* — least privilege applied to the AI resource itself, not just the data behind it.
- **Data-use guarantee** — prompts and completions sent to Azure OpenAI are **not** used to train the underlying foundation models and aren't shared with other customers — a specific, testable compliance/data-residency fact that distinguishes an enterprise Azure AI deployment from a public consumer AI product.

---

## Architecture

```mermaid
flowchart TD
    AI["AI adoption"] --> Attack["AI as attack surface<br/>(secure the AI workload)"]
    AI --> Defense["AI as a defense capability<br/>(AI-assisted security)"]

    Attack --> M365C["Microsoft 365 Copilot<br/>data oversharing risk"]
    Attack --> CustomAI["Custom AI apps<br/>(Azure AI Foundry)"]
    Attack --> AgentID["Agent identities"]

    M365C --> DSPM["Purview DSPM<br/>(AI observability)"]
    CustomAI --> AISPM["Defender for Cloud<br/>AI-SPM + AI threat protection<br/>(detect, out-of-band)"]
    CustomAI --> Native["AI-service-native controls:<br/>Content Safety/Prompt Shields (prevent, in-line),<br/>network isolation, CMK, RBAC"]
    AgentID --> Ent["Microsoft Entra Agent ID<br/>+ Conditional Access + Identity Governance"]

    Defense --> SecCopilot["Microsoft Security Copilot"]
    SecCopilot --> Embedded["Embedded: Defender XDR, Sentinel,<br/>Entra, Intune, Purview"]
    SecCopilot --> Standalone["Standalone portal"]
```

---

## Architecture Decisions

```mermaid
flowchart TD
    Q1["Deploying M365 Copilot or a genAI app on org data?"] -->|Yes| A1["Purview DSPM:<br/>data risk assessment first"]
    Q1 -->|No| Q2["Building/hosting a custom AI app?"]
    Q2 -->|Yes| A2["Defender for Cloud AI-SPM<br/>+ AI threat protection"]
    A2 --> Q2b["Need to block jailbreak/prompt injection<br/>in-line, before it reaches the model?"]
    Q2b -->|Yes| A2b["Content Safety + Prompt Shields<br/>(in-line, preventive)"]
    Q2 -->|No| Q3["Agent authenticates or acts autonomously?"]
    Q3 -->|Yes| A3["Microsoft Entra Agent ID<br/>+ Conditional Access + governance"]
    Q3 -->|No| Q4["Need faster SOC triage/investigation?"]
    Q4 -->|Yes| A4["Microsoft Security Copilot<br/>(embedded or standalone)"]
```

---

## Comparison

| Compare | Difference |
| --- | --- |
| Security Copilot vs. Microsoft 365 Copilot | SOC assistant embedded in Defender/Sentinel/Entra/Intune/Purview, billed via Security Compute Units (SCUs), vs. productivity assistant embedded in Office apps, licensed per user, inheriting the signed-in user's existing data permissions. Different products despite the shared name. |
| Defender for Cloud AI-SPM vs. AI threat protection | Posture: assesses configuration/attack paths *before* an incident, vs. runtime: detects active threats (prompt injection, jailbreak, data leakage, credential theft), feeding [[Microsoft Defender XDR]]. Same split as CSPM vs. CWPP elsewhere in [[Microsoft Defender for Cloud]]. |
| Microsoft Entra Agent ID vs. service principal / managed identity | Those authenticate an application or resource; Agent ID is purpose-built for autonomous agents — identity blueprints provision them at scale with parent-child relationships, governed by the same [[Conditional Access]] and Identity Governance controls used for human identities. |
| MCSB v2 AI Security domain vs. Defender for Cloud AI-SPM | [[Microsoft Cloud Security Benchmark (MCSB)|MCSB]] is the scored baseline defining what good AI security looks like; AI-SPM is the tool that assesses and enforces against it — same Plan/Monitor/Establish relationship MCSB has with every other domain. |
| Content Safety/Prompt Shields vs. Defender for Cloud AI threat protection | Content Safety/Prompt Shields runs **in-line**, inside the AI service's own request/response pipeline — it blocks harmful content or a jailbreak/injection attempt *before* the model ever processes it or a user ever sees the output. AI threat protection is **out-of-band** — it analyzes Azure resource telemetry after the fact and raises an alert in [[Microsoft Defender XDR]]. Same prevent-vs-detect split as AI-SPM vs. AI threat protection, one layer closer to the actual request. |

---

## AZ-500 Review

Not covered at all — Security Copilot, Copilot data risk tooling, Agent ID, Defender for Cloud's AI-specific posture/threat protection, and Azure AI services' own controls (Content Safety, Prompt Shields, AI Foundry network isolation/RBAC) all shipped after AZ-500's scope. Underlying mechanisms (Conditional Access, Purview, Defender for Cloud, Private Link, CMK, RBAC) are still AZ-500 fundamentals; only the AI-specific application of them is new.

---

## What's New for SC-100

- Split "securing AI" into two exam-tested problems: securing AI as a *workload* (data, custom apps, agents) vs. using AI as a *security capability* (Security Copilot).
- Know agent identity as a distinct, governable identity class in [[Entra ID]] — not interchangeable with service principals or managed identities — now reachable by [[Conditional Access]] and Identity Governance.
- Recognize Purview DSPM's Data Risk Assessment as the standard pre-Copilot-rollout answer to oversharing risk, not DLP alone.
- Know Defender for Cloud's AI-SPM/AI threat protection as the AI-specific extension of the CSPM/CWPP model already covered in [[Security Posture Assessments]] — same pattern, new workload type.
- Tie AI security decisions back to [[Cloud Adoption Framework (CAF)|CAF]]'s AI adoption lifecycle (Strategy → Plan → Ready → Govern → Manage → Secure) for strategy-level questions.
- Recognize Content Safety/Prompt Shields as the concrete, in-line answer to prompt-injection scenarios — Defender for Cloud's AI threat protection detects after the fact, it doesn't block the request itself.
- Apply the same PaaS network/identity/encryption pattern (Private Link, CMK, RBAC) used across the rest of the vault to Azure OpenAI/AI Foundry — it isn't a special case, just a new resource type wearing the same controls.

---

## Exam Tips

- "Assess data oversharing risk before a Copilot rollout" → Purview DSPM Data Risk Assessment, not Conditional Access or DLP alone.
- "Assistant embedded in Defender/Sentinel, capacity billed in compute units" → Security Copilot, not Microsoft 365 Copilot.
- "Authenticate and govern an autonomous AI agent at scale" → Microsoft Entra Agent ID with identity blueprints, not a shared service principal.
- Expect AI scenarios blended with an existing SC-100 topic (sensitivity labels, Conditional Access, Zero Trust) rather than AI tested in isolation.
- "Block a jailbreak or prompt injection attempt before it reaches the model" → Content Safety/Prompt Shields, not Defender for Cloud (which only detects after telemetry is emitted).
- "Restrict who can deploy/change a model vs. who can only call it" → RBAC scoped to the AI Foundry hub/project, not a broad Contributor role.
- "Encrypt fine-tuned model data with org-controlled keys" → customer-managed keys in Key Vault, the same CMK pattern as any other PaaS data store.

---

## Common Exam Confusion

- **Security Copilot vs. Microsoft 365 Copilot** — see Comparison table; the exam relies on candidates conflating "Copilot" as one product.
- **AI-SPM vs. AI threat protection** — posture/pre-incident vs. runtime/active-incident, same pattern as CSPM vs. CWPP.
- **Agent identity vs. workload identity** — agents are a governed *subset* of non-human identity, not a replacement term for service principals or managed identities.
- **Content Safety/Prompt Shields vs. Defender for Cloud AI threat protection** — in-line prevention inside the request pipeline vs. out-of-band detection after telemetry is emitted; see Comparison above.

---

## Keywords

- Microsoft Security Copilot, Security Compute Units (SCU), embedded vs. standalone experiences
- Microsoft 365 Copilot data security and compliance
- Microsoft Purview DSPM, AI observability, Data Risk Assessment, oversharing
- Microsoft Entra Agent ID, agent identity, identity blueprint
- AI security posture management (AI-SPM), AI threat protection
- Prompt injection, jailbreak, data leakage, data poisoning, credential theft
- Microsoft Agent 365, shadow AI / shadow agents
- MCSB v2 AI Security domain, CAF Secure AI adoption lifecycle
- Azure AI Content Safety, Prompt Shields
- Jailbreak (direct) vs. indirect prompt injection
- Azure AI Foundry managed virtual network, customer-managed keys (AI), AI Foundry RBAC

---

## Related Services

- [[Microsoft Cloud Security Benchmark (MCSB)]]
- [[Cloud Adoption Framework (CAF)]]
- [[Conditional Access]]
- [[Microsoft Defender for Cloud]]
- [[Microsoft Sentinel]]
- [[Purview]]
- [[Data Security Posture Management (DSPM)]]
- [[Security Posture Assessments]]
- [[Zero Trust]]
- [[Entra ID]]
- [[Microsoft Entra Agent ID]]
- [[Microsoft Security Copilot]]
- [[Threat Intelligence]]
- [[Securing Microsoft 365]]
- [[Threat Modeling]]
- [[Key Vault]]
- [[Private Link]]
- [[Securing IaaS and PaaS Services]]
- [[Data Classification and Protection]]

---

## References

- [What is Microsoft Security Copilot?](https://learn.microsoft.com/en-us/copilot/security/microsoft-security-copilot) — Microsoft Learn
- [What is Microsoft Entra Agent ID?](https://learn.microsoft.com/en-us/entra/agent-id/what-is-microsoft-entra-agent-id) — Microsoft Learn
- [Use Microsoft Purview to manage data security & compliance for Microsoft 365 Copilot](https://learn.microsoft.com/en-us/purview/ai-m365-copilot) — Microsoft Learn
- [Prevent oversharing with DSPM for AI data risk assessments](https://learn.microsoft.com/en-us/purview/data-security-posture-management-oversharing) — Microsoft Learn
- [AI security posture management - Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-security-posture) — Microsoft Learn
- [Secure AI - Cloud Adoption Framework](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ai/secure) — Microsoft Learn
- [Azure AI Content Safety overview](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/overview) — Microsoft Learn
- [Prompt Shields in Azure AI Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/jailbreak-detection) — Microsoft Learn
- [Azure AI Foundry network isolation](https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/configure-private-link) — Microsoft Learn
- [[Exam Objectives]]

---

## Verification Flag

Fast-moving surface — Entra Agent ID, MCSB v2's AI Security domain, and Microsoft Agent 365 were preview or recently-GA as of 2026-08-03. Re-verify GA status and licensing (especially SCU pricing) close to exam date. Also re-verify Azure AI Foundry's managed virtual network capabilities and exact RBAC role names for hub/project scoping — the AI Foundry platform (network isolation, role definitions) has been iterating quickly and naming may have shifted since this note was written (2026-08-08).
