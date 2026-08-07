---
tags:
  - sc100
type: concept
domain:
  - best-practices
  - apps-data
aliases:
  - DevSecOps
---

# DevOps Security

## Purpose

Centralizing pipeline and repository security findings — secrets, vulnerable code, vulnerable dependencies, misconfigured IaC — into the same Defender for Cloud posture view as cloud resources, via **Microsoft Defender for DevOps**.

---

## Why Architects Choose It

- Defender for DevOps is the concrete product that operationalizes [[Shift left (WAF)|shift left]]/DevSecOps — it connects GitHub and Azure DevOps organizations into [[Microsoft Defender for Cloud]] so pipeline/repo findings surface next to cloud resource findings in the same Secure Score and recommendations, instead of living in a separate DevOps-only tool.
- **Pull request annotations** are the literal enforcement mechanism that makes shift left concrete rather than aspirational — a finding (secret, vulnerable dependency, misconfigured template) is flagged inline in the PR, before merge, not discovered post-deployment.
- One connector covers four distinct attack surfaces a pipeline can leak through — secrets, first-party code, third-party dependencies, and infrastructure-as-code templates — each needing a different scanning technique.
- **GitHub Advanced Security for Azure Devops (GHAzDO)** extends the same CodeQL-based scanning engine GitHub-native repos already have to Azure DevOps repos — the licensing vehicle that closes the parity gap between the two platforms.

---

## Core Capabilities

- **Secret scanning** — detects committed credentials, keys, and tokens across repository history and pipeline logs, before they reach production or a public repo.
- **Static Application Security Testing (SAST)** — CodeQL semantic analysis of first-party code for known vulnerability patterns (injection, insecure deserialization, etc.).
- **Software Composition Analysis (SCA) / dependency scanning** — flags third-party/open-source packages with known CVEs, and can surface OSS license risk.
- **Infrastructure as Code (IaC) scanning** — checks Bicep/Terraform/ARM templates against misconfiguration rules *before* deployment — the same posture rules [[Security Posture Assessments|MCSB]] applies *after* deployment, shifted earlier. Container image scanning is the same shift-left idea applied to a container build — see [[Container and Kubernetes Security]] for the registry-scan-to-admission-control pipeline it feeds into.
- **Pull request annotations** — inline PR comments surface findings from all four scan types at the exact point a merge decision is made.
- **Centralized findings** — DevOps security recommendations feed the same Secure Score and attack-path view in Defender for Cloud that cloud resource findings do (see [[CSPM and CWPP]]).

---

## Architecture

```mermaid
flowchart LR
    Repo["GitHub / Azure DevOps repo"] --> Connector["Defender for DevOps connector"]
    Connector --> Secrets["Secret scanning"]
    Connector --> SAST["SAST (CodeQL)"]
    Connector --> SCA["SCA / dependency scanning"]
    Connector --> IaC["IaC template scanning"]

    Secrets --> PR["Pull request annotations<br/>(pre-merge)"]
    SAST --> PR
    SCA --> PR
    IaC --> PR

    Secrets --> DfC["Defender for Cloud<br/>Secure Score / recommendations"]
    SAST --> DfC
    SCA --> DfC
    IaC --> DfC
```

---

## Architecture Decisions

```mermaid
flowchart TD
    Q1["Repos hosted on GitHub or Azure DevOps?"] -->|GitHub| A1["Connect GitHub connector;<br/>GitHub Advanced Security native"]
    Q1 -->|Azure DevOps| A2["Connect Azure DevOps connector;<br/>needs GHAzDO for CodeQL/secret scanning parity"]
    A1 --> Q2["Need findings blocked pre-merge,<br/>not just centrally visible?"]
    A2 --> Q2
    Q2 -->|Yes| A3["Enable PR annotations"]
    Q2 -->|No| A4["Findings still centralize into<br/>Defender for Cloud Secure Score"]
```

---

## Comparison

| Compare | Difference |
| --- | --- |
| Defender for DevOps vs. Defender for Cloud CSPM/CWPP | Defender for DevOps scans source/pipeline *before* deployment (pre-prod, shift-left stage). CSPM assesses and CWPP protects the resource *after* deployment (runtime). Same Defender for Cloud console and Secure Score, different lifecycle stage — see [[Shift left (WAF)]] for the shift-left/shift-right framing this maps to. |
| SAST vs. SCA | SAST (CodeQL) analyzes the organization's *own* code for vulnerability patterns. SCA analyzes *third-party/open-source* dependencies for known CVEs. Different attack surface — a "vulnerable package version" scenario is SCA, a "custom code has an injection flaw" scenario is SAST. |
| GitHub Advanced Security vs. GHAzDO | Same underlying CodeQL/secret-scanning engine; GitHub Advanced Security is native to GitHub-hosted repos, GHAzDO is the licensing product that brings the identical capability to Azure DevOps-hosted repos. A naming/platform distinction, not a capability difference. |
| Defender for DevOps vs. threat modeling | Threat modeling (see [[Threat Modeling]]) reasons about hypothetical design-time threats before code exists; Defender for DevOps scans *actual* code/dependencies/IaC once they exist in a repo. Threat modeling's output (requirements) precedes what Defender for DevOps then continuously verifies. |

---

## AZ-500 Review

AZ-500 does not cover pipeline or repository security at all — secret scanning, SAST, SCA, and IaC scanning are entirely new territory for SC-100, the same framing as [[Shift left (WAF)]].

---

## What's New for SC-100

- Recommend Defender for DevOps by name as the mechanism that centralizes pipeline/repo findings into the same posture view as cloud resources — not a separate DevOps-only tool.
- Know PR annotations as the specific pre-merge enforcement point that operationalizes shift left, rather than describing shift left only as a principle.
- Map a described leak or vulnerability to the correct scanning category (secrets/SAST/SCA/IaC) — a frequent scenario-matching skill.
- Know GHAzDO as the Azure DevOps equivalent licensing vehicle for GitHub Advanced Security — closing platform parity is a named, testable fact.

---

## Exam Tips

- "Credentials committed to a repository" → secret scanning, not SAST.
- "Known-vulnerable open-source package version" → SCA/dependency scanning, not SAST.
- "Misconfigured Terraform/Bicep template caught before deployment" → IaC scanning, feeding the same posture rules as MCSB.
- "Findings from both GitHub and Azure DevOps need one unified security view alongside cloud posture" → Defender for DevOps.
- Azure DevOps repos needing CodeQL-based scanning parity with GitHub → GHAzDO license, not a manual/custom pipeline task.

---

## Common Exam Confusion

- **SAST vs. SCA** — own code vs. third-party dependencies; full comparison above.
- **Defender for DevOps vs. CSPM/CWPP** — pre-deployment pipeline scanning vs. post-deployment resource assessment/protection.
- **GitHub Advanced Security vs. GHAzDO** — same engine, different host platform.
- **Defender for DevOps vs. threat modeling** — verifying what exists vs. reasoning about what doesn't exist yet.

---

## Keywords

- Microsoft Defender for DevOps
- GitHub Advanced Security for Azure DevOps (GHAzDO)
- Secret scanning
- Static Application Security Testing (SAST), CodeQL
- Software Composition Analysis (SCA), dependency scanning
- Infrastructure as Code (IaC) scanning
- Pull request annotations
- DevSecOps

---

## Related Services

- [[Shift left (WAF)]]
- [[Threat Modeling]]
- [[Security Posture Assessments]]
- [[CSPM and CWPP]]
- [[Azure Policy]]
- [[Microsoft Defender for Cloud]]
- [[Cloud Adoption Framework (CAF)]]
- [[Container and Kubernetes Security]]

---

## References

- [Defender for DevOps overview](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-devops-introduction) — Microsoft Learn
- [GitHub Advanced Security for Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/repos/security/configure-github-advanced-security-features) — Microsoft Learn
- [[Exam Objectives]]
