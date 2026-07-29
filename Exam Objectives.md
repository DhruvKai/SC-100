---
tags:
  - sc100
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

### Design a resiliency strategy for ransomware and other attacks (Microsoft Security Best Practices)

- Security strategy for business resiliency, including prioritizing threats to business-critical assets
- BCDR for hybrid and multicloud, including secure backup and restore
- Ransomware mitigation, including BCDR and privileged access prioritization
- Evaluate solutions for security updates

### Align with MCRA and MCSB

- Best practices for cybersecurity capabilities and controls
- Protection against insider, external, and supply chain attacks
- AI solutions aligned to Microsoft Cloud Security Benchmark
- Zero Trust adoption framework alignment

### Align with Cloud Adoption Framework (CAF) and Well-Architected Framework (WAF)

- Strategy for secure AI adoption
- Security/governance strategy based on CAF and WAF
- Security and governance via Azure landing zones
- DevSecOps process aligned with CAF

---

## 2. Security Operations, Identity, and Compliance (25–30%)

### Security operations

- Detection and response: [[Microsoft Defender XDR]] and [[Microsoft Sentinel]] (XDR/SIEM)
- Centralized logging and auditing, including Microsoft Purview Audit ([[Purview]])
- Monitoring across hybrid and multicloud
- SOAR solution using [[Microsoft Sentinel]] and [[Microsoft Defender XDR]]
- Incident response, threat hunting, incident management workflows
- Threat detection coverage via MITRE ATT&CK (Enterprise, Mobile, ICS)

### Identity and access management

- Agent identities via Microsoft Entra Agent ID and [[Conditional Access]] policies
- Access to SaaS, PaaS, IaaS, hybrid/on-prem, multicloud (identity, network, app controls)
- [[Entra ID]] solution for hybrid/multicloud
- External identities: B2B and decentralized identity
- Modern authN/authZ strategy: [[Conditional Access]], continuous access evaluation, risk scoring, protected actions
- Validate Conditional Access alignment with Zero Trust
- Harden Active Directory Domain Services (AD DS)
- Secrets, keys, certificates management ([[Key Vault]])

### Securing privileged access

- Privileged role assignment/delegation via enterprise access model
- Security/governance of [[Entra ID]], including [[PIM]], entitlement management, access reviews
- Security/governance of AD DS, resilience to common attacks
- Securing cloud tenant administration (SaaS + multicloud)
- Cloud infrastructure entitlement management (CIEM)
- Access review management solution evaluation
- Secure workstations for privileged access, including remote access

### Regulatory compliance

- Translate compliance requirements into security controls
- Compliance via [[Purview]]
- [[Azure Policy]] solutions for security/compliance
- Alignment with regulatory standards/benchmarks via [[Microsoft Defender for Cloud]]

---

## 3. Security Solutions for Infrastructure (25–30%)

### Security posture management (hybrid/multicloud)

- Posture evaluation via [[Microsoft Defender for Cloud]] and MCSB
- Microsoft Secure Score evaluation
- Integrated posture management across hybrid/multicloud
- Cloud workload protection plans in [[Microsoft Defender for Cloud]]
- Hybrid/multicloud integration via [[Azure Arc]]
- Microsoft Defender External Attack Surface Management (Defender EASM)
- Microsoft Security Exposure Management: attack paths, attack surface reduction, initiatives

### Server and client endpoint security

- Server security across platforms/OS
- Mobile device/client endpoint protection, hardening, configuration
- IoT and embedded system security
- OT/ICS security via Microsoft Defender for IoT
- Security baselines for server/client endpoints
- Windows LAPS evaluation

### SaaS, PaaS, IaaS security requirements

- Security baselines for SaaS/PaaS/IaaS
- IoT workload security requirements
- Web workload security requirements
- Container security requirements
- Container orchestration security requirements
- Azure AI services security

### Network security and Security Service Edge (SSE)

- Network design evaluation against security requirements
- Microsoft Entra Internet Access as secure web gateway
- Microsoft Entra Internet Access for Microsoft services (cross-tenant)
- Microsoft Entra Private Access

---

## 4. Security Solutions for Applications and Data (20–25%)

### Securing Microsoft 365

- Productivity/collaboration security posture (Secure Score)
- Microsoft Defender for Office 365 and Microsoft Defender for Cloud Apps
- Device management via [[Intune]]
- Data security in Microsoft 365 via [[Purview]]
- Data security/compliance in Microsoft Copilot for Microsoft 365

### Securing applications

- Security posture of existing application portfolios
- Threat modeling for business-critical applications
- Full lifecycle application security strategy
- Standards/practices for secure application development
- Map technologies to application security requirements
- Workload identities for authenticating/accessing Azure resources
- API management and security
- [[Azure Web Application Firewall]] (WAF)

### Securing organizational data

- Data discovery and classification
- Prioritize threat mitigation for data
- Encryption at rest/in transit, including [[Key Vault]] and infrastructure encryption
- Data security for AI workloads
- Data security for Azure SQL, Azure Synapse Analytics, Azure Cosmos DB
- Data security for Azure Storage
- Microsoft Defender for Storage and Microsoft Defender for Databases

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

---

## Verification Flag

Content is a verbatim summary of the official Microsoft Learn study guide fetched 2026-07-29. Re-verify before relying on it if your exam date is far in the future — Microsoft revises skills-measured outlines periodically (see "Updates to the exam" on the source page).
