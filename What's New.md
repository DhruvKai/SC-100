---
tags:
  - sc100
type: index
aliases:
  - Changelog
  - Change Log
---

# What's New

Changelog of vault additions and significant updates, newest first. One entry per push to GitHub.

Maintained per the Changelog rule in `CLAUDE.md` — update this page in the same commit as the change it describes.

---

## 2026-09-14

**Added**

- [[Privileged Access Tier Models]] — deep dive on the legacy AD Tier 0/1/2 model vs. the Enterprise Access Model (Control/Management/Data-Workload planes, User/App/Privileged access pathways), plus the Enterprise/Specialized/Privileged device security levels.
- [[AdminSDHolder and SDProp]] — the AD DS object/process that enforces ACLs on protected groups hourly, the orphaned `adminCount=1` cleanup problem, and its use as a Defender-for-Identity-detected attacker persistence technique.
- [[Purview Compliance Manager]] — Compliance Manager's control/assessment/group/regulatory-template object model, groups' permanence and lack of security boundary, and shared improvement actions.
- [[SQL Data Protection (TDE, Ledger, TDS 8.0)]] — TDE (service-managed vs. customer-managed/BYOK, revocation/rotation timing), Ledger (Merkle tree tamper-evidence, updatable vs. append-only tables), and TDS 8.0 strict encryption (`Encrypt=strict`, TLS before any TDS data).
- [[Double Key Encryption (DKE)]] — the two-key, Microsoft-can't-decrypt architecture for the highest-sensitivity data tier, and the collaboration-feature trade-offs it forces.
- [[Assigning Regulatory Compliance Standards]] — the exact portal workflow (scope, permissions, automated vs. manual assessments) for turning a named regulation into a tracked compliance percentage in Defender for Cloud, and its integration with Purview Compliance Manager.
- [[Defender for Cloud REST API]] — `Microsoft.Security` operation groups and the landing-zone-scale automation use cases they enable (Pricings, Standard Assignments, Secure Scores, Alerts, Governance Rules, multicloud/DevOps connectors).

**Updated**

- [[Securing Privileged Access]], [[Securing Active Directory Domain Services (AD DS)]], [[Identity and Access Management (IAM)]] — cross-linked to [[Privileged Access Tier Models]] and [[AdminSDHolder and SDProp]].
- [[Purview]], [[Compliance and Privacy]] — cross-linked to [[Purview Compliance Manager]].
- [[Data Classification and Protection]], [[Key Vault]] — cross-linked to [[SQL Data Protection (TDE, Ledger, TDS 8.0)]] and [[Double Key Encryption (DKE)]].
- [[Security Posture Assessments]], [[Security Scoring Dashboards]], [[Purview Compliance Manager]] — cross-linked to [[Assigning Regulatory Compliance Standards]] and [[Defender for Cloud REST API]].
- [[services|Services]], [[Architecture Decisions]] — rows for all seven new notes.

---

## 2026-09-10

**Added**

- [[MARS Agent]] — Microsoft Azure Recovery Services agent: direct-to-vault Windows backup of files/folders/volume/system state, full OS compatibility matrix (64-bit only, no Server Core, no Linux), the passphrase / security PIN / soft delete security model, and MARS vs. MABS/DPM vs. VM-extension decisions.

**Updated**

- [[Microsoft 365 Licensing]] — new "Entra ID Protection: capabilities by tier" table (Free/P1 give only *limited* risk reports; risk policies and risk-based Conditional Access need P2), plus matching exam tip and confusion pair.
- [[Identity Protection]] — added a License Requirements section spelling out the Free = P1 reporting limits and the P2-only capabilities; linked to [[Microsoft 365 Licensing]].
- [[services|Services]], [[Architecture Decisions]] — rows for [[MARS Agent]] and the MARS-vs-MABS/DPM decision.
- [[Ransomware Resiliency and BCDR]], [[Resource Guard]], [[Securing Active Directory Domain Services (AD DS)]] — cross-linked to [[MARS Agent]] (DC system state backup, security PIN as the MUA predecessor).

---

## 2026-08-31

**Added**

- [[Microsoft 365 Licensing]] — Office 365 vs. Microsoft 365 vs. EMS, E3 vs. E5, the E5 Security / E5 Compliance add-ons, Business Premium and frontline SKUs, and which security products no M365 license includes.
- [[Resource Guard]] — Multi-User Authorization (MUA) for Azure Backup and Site Recovery: protected operations, cross-subscription/cross-tenant placement, and the JIT approval flow via [[PIM]].
- [[Microsoft Incident Response (DART)]] — DART and CRSP under the Microsoft Incident Response brand, reactive vs. proactive services, retainers, and the ransomware investigation sequence.
- [[Rapid Modernization Plan (RaMP)]] — Zero Trust's execution layer: initiative checklists, named accountable/responsible owners, and the privileged access RaMP's 30/90/beyond staging.
- [[Playbooks and Automation Rules]] — Sentinel automation rules vs. playbooks vs. Defender XDR native response, plus the IR-playbook terminology collision.
- [[Securing Active Directory Domain Services (AD DS)]] — AD DS security requirements and attack surface reduction: privileged group hygiene, PAWs and tier boundaries, DC hardening, gMSA/LAPS/delegation, and Defender for Identity detection.
- [[Secure Score Mechanics]] — how both Secure Scores are calculated: Defender for Cloud's control weighting and `(max ÷ resources) × healthy` formula vs. Microsoft Secure Score's improvement-action points and status effects.
- This page, plus a Changelog rule in `CLAUDE.md`.

**Updated**

- [[services|Services]], [[Architecture Decisions]], [[Frameworks Cheat Sheet]] — rows and decision shortcuts for all seven new notes; RaMP added to the framework relations diagram and confusion pairs.
- [[Ransomware Resiliency and BCDR]] — MUA/[[Resource Guard]] added to the Prepare phase, comparison table, and exam tips.
- [[Zero Trust]], [[Security Adoption Framework (SAF)]] — linked to [[Rapid Modernization Plan (RaMP)]] as the sequencing layer.
- [[Security Operations]], [[Microsoft Sentinel]], [[Logic Apps]] — automation-layer breakdown linked to [[Playbooks and Automation Rules]].
- [[Identity and Access Management (IAM)]], [[Securing Privileged Access]], [[Entra ID]], [[Securing Server and Client Endpoints]] — linked to the new AD DS note.
- [[Security Posture Assessments]], [[Security Scoring Dashboards]], [[Securing Microsoft 365]] — linked to [[Secure Score Mechanics]] and [[Microsoft 365 Licensing]].
- [[00 Home]] — links to this changelog.

---

## 2026-08-28

**Added**

- [[External Attack Surface Management (EASM)]] — outside-in discovery of unknown internet-facing assets: subdomain takeover, certificate sprawl, DNS hygiene, shadow IT, post-M&A exposure.
- [[Shared Responsibility Model]] — customer/Microsoft ownership split across on-prem, IaaS, PaaS, and SaaS.
- [[OT and ICS Security]] — Defender for IoT OT edition, passive sensors, Purdue Model zone/conduit segmentation.
- [[Attack Chain Models]] — Lockheed Martin Cyber Kill Chain vs. MITRE ATT&CK, with NotPetya walked through both.

**Updated**

- [[CSPM and CWPP]], [[Security Posture Assessments]], [[DevOps Security]] — EASM integrated into the CNAPP picture.
- [[AI and Copilot Security Architecture]] — AI-specific shared responsibility.
- [[Network Security Architecture]], [[Securing IaaS and PaaS Services]], [[Securing Server and Client Endpoints]], [[Security Operations]] — OT/ICS and shared responsibility cross-links.
- [[Azure Well-Architected Framework (WAF)]] — Security pillar 12-point checklist and antipatterns.
- [[Cloud Adoption Framework (CAF)]] — Govern methodology, policy MVP, Five Disciplines of Cloud Governance.
- [[Zero Trust]] — common adoption antipatterns.
- [[Frameworks Cheat Sheet]] — "How the Frameworks Relate" diagram.
- [[Threat Intelligence]], [[Threat Modeling]] — attack chain cross-links.

---

## 2026-08-22

**Updated**

- [[Frameworks Cheat Sheet]] — decision shortcut table linked through to each framework note.

---

## 2026-08-21

**Updated**

- [[services|Services]] — Global Secure Access, Entra Private Access (ZTNA), and Entra Internet Access rows added under [[Identity as the Security Perimeter]].

---

## 2026-08-19

**Added**

- [[Priva]] — Subject Rights Requests and Privacy Risk Management, scoped to the Microsoft 365 data estate.

**Updated**

- [[API Management and Security]] — classic vs. v2 tiers and the VNet integration boundary.
- [[Purview]], [[Compliance and Privacy]], [[Data Classification and Protection]], [[Data Security Posture Management (DSPM)]] — Priva cross-links and the Purview/Priva boundary.
- [[00 Home]], [[Exam Objectives]] — link fixes.

---

## Related

- [[00 Home]]
- [[Exam Objectives]]
- [[Architecture Decisions]]
- [[services|Services]]
