---
tags:
  - sc100
type: index
status: needs-verification
---

# Exam Objectives

## Purpose

Official SC-100 skills-measured outline, kept verbatim from Microsoft Learn for scope-checking notes against the exam.

---

## Source

- [Study guide for Exam SC-100](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/sc-100) — Microsoft Learn
- Skills measured as of **July 28, 2026** (page last updated 2026-06-29)
- Passing score: 700+

---

## Skills at a Glance

| Domain | Weight |
| --- | --- |
| Design solutions that align with security best practices and priorities | 20–25% |
| Design security operations, identity, and compliance capabilities | 25–30% |
| Design security solutions for infrastructure | 25–30% |
| Design security solutions for applications and data | 20–25% |

---

## 1. Security Best Practices and Priorities (20–25%)

### Design a [[Ransomware Resiliency and BCDR|resiliency strategy for ransomware]] and other attacks (Microsoft Security Best Practices)

- Security strategy for business resiliency, including prioritizing threats to business-critical assets
- BCDR for hybrid and multicloud, including secure backup and restore
- Ransomware mitigation, including BCDR and privileged access prioritization
- [[Ransomware Resiliency and BCDR|Evaluate solutions for security updates]] (Azure Update Manager)

### Align with [[Microsoft Cybersecurity Reference Architectures (MCRA)|MCRA]] and [[Microsoft Cloud Security Benchmark (MCSB)|MCSB]]

- Best practices for cybersecurity capabilities and controls
- Protection against insider, external, and supply chain attacks
- [[AI and Copilot Security Architecture|AI solutions aligned to Microsoft Cloud Security Benchmark]]
- [[Zero Trust]] adoption framework alignment

### Align with [[Cloud Adoption Framework (CAF)]] and [[Azure Well-Architected Framework (WAF)]]

- [[AI and Copilot Security Architecture|Strategy for secure AI adoption]]
- Security/governance strategy based on [[Cloud Adoption Framework (CAF)]] and [[Azure Well-Architected Framework (WAF)]]
- Security and governance via [[Azure Landing Zones|Azure landing zones]]
- [[DevOps Security|DevSecOps process aligned with Cloud Adoption Framework (CAF)]]

---

## 2. Security Operations, Identity, and Compliance (25–30%)

### [[Security Operations]]

- Detection and response: [[Microsoft Defender XDR]] and [[Microsoft Sentinel]] ([[Security Operations|XDR/SIEM]])
- Centralized [[Azure Security Logging|logging]] and auditing, including Microsoft Purview Audit ([[Purview]])
- [[Security Operations|Monitoring across hybrid and multicloud]]
- [[Security Operations|SOAR solution using Microsoft Sentinel and Microsoft Defender XDR]]
- [[Security Operations|Incident response, threat hunting, incident management workflows]]
- [[Security Operations|Threat detection coverage via MITRE ATT&CK (Enterprise, Mobile, ICS)]]

### Identity and access management

- [[AI and Copilot Security Architecture|Agent identities via Microsoft Entra Agent ID]] and [[Conditional Access]] policies
- [[Identity and Access Management (IAM)|Access to SaaS, PaaS, IaaS, hybrid/on-prem, multicloud]] (identity, network, [[SaaS Application Discovery and Control|app controls]])
- [[Entra ID]] solution for hybrid/multicloud ([[Identity as the Security Perimeter|IdP architecture]])
- [[Identity as the Security Perimeter|External identities: B2B and decentralized identity]]
- Modern authN/authZ strategy: [[Conditional Access]], continuous access evaluation, risk scoring, protected actions
- Validate [[Conditional Access]] alignment with [[Zero Trust]]
- [[Identity and Access Management (IAM)|Harden Active Directory Domain Services (AD DS)]]
- [[Identity and Access Management (IAM)|Secrets, keys, certificates management]] ([[Key Vault]])

### [[Securing Privileged Access]]

- [[Securing Privileged Access|Privileged role assignment/delegation via enterprise access model]]
- Security/governance of [[Entra ID]], including [[PIM]], [[Securing Privileged Access|entitlement management, access reviews]]
- [[Securing Privileged Access|Security/governance of AD DS, resilience to common attacks]]
- [[Securing Privileged Access|Securing cloud tenant administration (SaaS + multicloud)]]
- [[Securing Privileged Access|Cloud infrastructure entitlement management (CIEM)]]
- [[Securing Privileged Access|Access review management solution evaluation]]
- [[Securing Privileged Access|Secure workstations for privileged access, including remote access]]

### [[Compliance and Privacy|Regulatory compliance]]

- Translate compliance requirements into security controls
- Compliance via [[Purview]]
- [[Azure Policy]] solutions for security/compliance
- Alignment with regulatory standards/benchmarks via [[Microsoft Defender for Cloud]]

---

## 3. Security Solutions for Infrastructure (25–30%)

### [[Security Posture Assessments|Security posture management]] (hybrid/multicloud)

- Posture evaluation via [[Microsoft Defender for Cloud]] and MCSB
- [[Security Scoring Dashboards|Microsoft Secure Score]] evaluation
- Integrated posture management across hybrid/multicloud
- [[Cloud Workload Protection (CWPP)|Cloud workload protection plans]] in [[Microsoft Defender for Cloud]]
- [[Azure Arc|Hybrid/multicloud integration via Azure Arc]]
- [[CSPM and CWPP|Microsoft Defender External Attack Surface Management (Defender EASM)]]
- Microsoft Security Exposure Management: attack paths, attack surface reduction, initiatives

### [[Securing Server and Client Endpoints]]

- [[Securing Server and Client Endpoints|Server security across platforms/OS]]
- [[Securing Server and Client Endpoints|Mobile device/client endpoint protection, hardening, configuration]]
- [[Securing Server and Client Endpoints|IoT and embedded system security]]
- [[Securing Server and Client Endpoints|OT/ICS security via Microsoft Defender for IoT]]
- [[Securing Server and Client Endpoints|Security baselines for server/client endpoints]]
- [[Securing Server and Client Endpoints|Windows LAPS evaluation]]

### SaaS, PaaS, IaaS security requirements

- [[Securing IaaS and PaaS Services|Security baselines for SaaS/PaaS/IaaS]]
- [[Securing Server and Client Endpoints|IoT workload security requirements]]
- [[Securing IaaS and PaaS Services|Web workload security requirements]]
- [[Container and Kubernetes Security|Container security requirements]]
- [[Container and Kubernetes Security|Container orchestration security requirements]]
- [[AI and Copilot Security Architecture|Azure AI services security]]

### [[Identity as the Security Perimeter|Network security and Security Service Edge (SSE)]]

- [[Network Security Architecture|Network design evaluation against security requirements]]
- [[Identity as the Security Perimeter|Microsoft Entra Internet Access as secure web gateway]]
- [[Identity as the Security Perimeter|Microsoft Entra Internet Access for Microsoft services (cross-tenant)]]
- [[Identity as the Security Perimeter|Microsoft Entra Private Access]]

---

## 4. Security Solutions for Applications and Data (20–25%)

### [[Securing Microsoft 365]]

- [[Securing Microsoft 365|Productivity/collaboration security posture (Secure Score)]]
- [[Securing Microsoft 365|Microsoft Defender for Office 365]] and [[SaaS Application Discovery and Control|Microsoft Defender for Cloud Apps]]
- Device management via [[Intune]]
- Data security in Microsoft 365 via [[Purview]]
- [[AI and Copilot Security Architecture|Data security/compliance in Microsoft Copilot for Microsoft 365]]

### Securing applications

- Security posture of existing application portfolios
- [[Threat Modeling|Threat modeling for business-critical applications]]
- Full lifecycle application security strategy
- [[DevOps Security|Standards/practices for secure application development]]
- Map technologies to application security requirements
- [[Identity and Access Management (IAM)|Workload identities for authenticating/accessing Azure resources]]
- [[API Management and Security|API management and security]]
- [[Azure Web Application Firewall]] (WAF)

### Securing organizational data

- [[Data Security Posture Management (DSPM)|Data discovery and classification]]
- [[Data Security Posture Management (DSPM)|Prioritize threat mitigation for data]]
- [[Data Classification and Protection|Encryption at rest/in transit, including Key Vault and infrastructure encryption]]
- [[AI and Copilot Security Architecture|Data security for AI workloads]]
- [[Data Security Posture Management (DSPM)|Data security for Azure SQL, Azure Synapse Analytics, Azure Cosmos DB]]
- [[Data Security Posture Management (DSPM)|Data security for Azure Storage]]
- [[Cloud Workload Protection (CWPP)|Microsoft Defender for Storage and Microsoft Defender for Databases]]

---

## Exam Tips

- Weighting favors "Security Operations, Identity, and Compliance" and "Infrastructure" domains (25–30% each) — prioritize study time accordingly.
- Questions are scenario-based architecture decisions, not feature trivia — expect "recommend a solution for..." phrasing.
- Most questions target GA features; Preview features appear only if commonly used in practice.

---

## Related Services

- [[Microsoft Sentinel]]
- [[Microsoft Defender for Cloud]]
- [[Entra ID]]
- [[Conditional Access]]
- [[PIM]]
- [[Purview]]
- [[Key Vault]]
- [[Azure Policy]]
- [[Azure Arc]]
- [[AI and Copilot Security Architecture]]
- [[Container and Kubernetes Security]]
- [[API Management and Security]]
- [[Trusted Platform Module (TPM)]]
- [[Securing Microsoft 365]]

---

## Verification Flag

Content is a verbatim summary of the official Microsoft Learn study guide fetched 2026-07-29. Re-verify before relying on it if your exam date is far in the future — Microsoft revises skills-measured outlines periodically (see "Updates to the exam" on the source page).
