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

## 2026-09-30

**Added**

- [[Security Updates and Patch Management]] — hub for the "evaluate solutions for security updates" Learn unit: four-area patch strategy, cloud-native vs. traditional vs. third-party approaches, CSPM integration, and AI-workload patching considerations.
- [[Azure Update Manager]] — native Windows/Linux server patching for Azure and Arc machines, plus the retired Automation Update Management + Configuration Manager integration (exam scenario: no WSUS and updates not from ConfigMgr → give the Windows agent Microsoft Update access).
- [[WSUS]] — upstream/downstream and autonomous/replica modes, GPO client configuration, deprecation status, and its role as an Azure Update Manager source for third-party updates.
- [[Microsoft Configuration Manager]] — SCCM/MECM components (Software Update Point, ADRs, distribution points), co-management, and when it loses to Azure Update Manager/Intune.
- [[Defender for Cloud Recommendations]] — recommendation anatomy (MITRE ATT&CK tactics/techniques, severity vs. risk level, Quick fix, exempt, governance, Enforce/Deny), with the "management ports → review the recommendation" exam scenario.
- [[Azure Resource Graph]] — cross-subscription KQL querying of posture data, and why it isn't the answer for exploit analysis.
- [[Confidential Computing Enclaves]] — Intel SGX enclaves (DCsv2/DCsv3 EPC limits, trusted/untrusted partitioning, attestation, SDKs) vs. confidential VMs, confidential containers, and Always Encrypted with secure enclaves.
- [[Microsoft Defender for Cloud]] — CNAPP hub page (previously the vault's most-linked missing note): capability map pointing to every CSPM/CWPP/DSPM/DevOps/CIEM/EASM note, Defender for Servers P1/P2 features, and Defender for Cloud vs. XDR/Sentinel/Policy.
- [[Azure Web Application Firewall]] — hosting options (Front Door, Application Gateway, App Gateway for Containers, CDN preview), Detection vs. Prevention, managed/custom/bot rules, and WAF vs. Azure Firewall/DDoS/APIM.
- [[Microsoft Entra Agent ID]] — agent identities and blueprints, CA/ID Protection/governance for agents, and the Agent 365 licensing split.
- [[Microsoft Azure Backup Server (MABS)]] — MABS v4 disk-to-disk-to-Azure architecture, domain/dedicated-server requirements, and MABS vs. MARS vs. DPM.
- [[01 Security Strategy]], [[02 Identity]], [[03 Infrastructure]], [[04 Applications]], [[05 Data]], [[06 Governance]] — section index pages linked from Home (previously blank/missing), each listing its notes and related service pages.

**Updated**

- [[Microsoft 365 Licensing]] — added a "Keep in Mind" E3 vs. E5 cheat sheet (E3 = productivity + basic identity + device management; E5 = E3 + advanced security/compliance/identity), with a per-pillar table and the E5-vs-E5 Security add-on trap. Also updated the note for 2026 repackaging: Defender for Office 365 P1 in E3, Intune Suite features rolling into E3/E5, and the Security Copilot SCU allowance in E5.
- [[Ransomware Resiliency and BCDR]] — Prevent-phase patching now links to [[Azure Update Manager]] and [[Security Updates and Patch Management]].
- [[Securing Server and Client Endpoints]] — server patch-compliance guidance linked to Azure Update Manager, with legacy ConfigMgr/WSUS options.
- [[Intune]] — Configuration Manager, WSUS and server-patching references now point to their own notes.
- [[Azure Arc]] — Update Manager link, plus ConfigMgr as an onboarding-at-scale path.
- [[Exam Objectives]] — security-updates objective re-pointed to the new patching hub; recommendations/ARG and encryption-in-use entries added.
- [[Security Posture Assessments]] — related links to the recommendation-anatomy and Resource Graph notes.
- [[Secure Score Mechanics]] — linked the individual recommendations the score is built from.
- [[Azure Policy]] — linked recommendations (Enforce/Deny), Update Manager (assessment policies) and Resource Graph.
- [[Attack Chain Models]] — linked Defender for Cloud recommendations as a posture-side MITRE ATT&CK mapping, and patching as the MS17-010 control.
- [[Securing IaaS and PaaS Services]] — linked the management-ports/JIT recommendation scenario and confidential computing.
- [[External Attack Surface Management (EASM)]] — Azure Resource Graph is now a wiki link.
- [[Data Classification and Protection]] — encryption-in-use row now links to confidential computing enclaves.
- [[Zero Trust]] — data-pillar Confidential Computing reference linked to its new note.
- [[SQL Data Protection (TDE, Ledger, TDS 8.0)]] — linked Always Encrypted with secure enclaves and Confidential Ledger context.
- [[Trusted Platform Module (TPM)]] — Trusted Launch vs. confidential VMs/enclaves cross-link.
- [[Key Vault]] — Managed HSM as an Azure confidential service, attestation-gated key release.
- [[Secure Future Initiative (SFI)]] — Microsoft's internal use of confidential computing under SFI.
- [[00 Home]] — added Exam Objectives and Azure Services Index links.
- [[Key Vault]] — `Managed HSM` alias so the services-index link resolves.
- [[Network Security Group]] — `ASG` / `Application Security Group` aliases so the services-index link resolves.
- [[Frameworks Cheat Sheet]] — fixed a WAF comparison link that pointed at Azure Firewall instead of Azure Web Application Firewall.
- [[Ransomware Resiliency and BCDR]] — MABS/DPM comparison row now links to the new MABS note.

---

## 2026-09-23

**Added**

- [[OAuth 2.0 and OpenID Connect]] — the four OAuth 2.0 roles (resource owner, client, authorization server, resource server), the authorization code flow, and OAuth (authorization) vs. OIDC (authentication) as separate protocols producing different tokens.
- [[VPN Gateway]] — Site-to-Site vs. Point-to-Site connectivity and the P2S protocol-to-client-OS support matrix (OpenVPN/SSTP/IKEv2), plus where VPN Gateway sits relative to Entra Private Access as the Zero Trust replacement.
- [[App Control for Business and AppLocker]] — kernel-mode vs. user-mode Windows application allowlisting, the Intelligent Security Graph reputable-apps model, and the per-user/legacy-OS scenarios that specifically point to AppLocker over App Control for Business.
- [[Workload Identity Federation]] — federated credential mechanics (issuer/subject/audience) for non-Azure-hosted workloads (Kubernetes pods, CI/CD, other clouds), and why it beats both managed identity (Azure-hosted only) and self-signed certificates (still a stored credential) outside their scope.

**Updated**

- [[Secure Score Mechanics]] — no content change; used to walk through the "highest secure score impact" control-weighting exam pattern (MFA/management ports over vulnerability/patch remediation).
- [[Securing Server and Client Endpoints]] — added a control-selection table for "protect client data accessed on/stored on mobile devices" (App protection policies + Conditional Access vs. UEBA/App Control for Business/user risk policy distractors), and cross-linked [[App Control for Business and AppLocker]].
- [[Identity and Access Management (IAM)]] — cross-linked [[OAuth 2.0 and OpenID Connect]] (delegated/application permissions as the OAuth flow's output) and [[Workload Identity Federation]] (federation mechanics detail).
- [[Entra ID]] — cross-linked [[OAuth 2.0 and OpenID Connect]] from the OIDC/OAuth-based SSO section.
- [[Container and Kubernetes Security]] — cross-linked [[Workload Identity Federation]] for general federation mechanics beyond the AKS-specific case.
- [[Data Classification and Protection]] — added Microsoft Purview Message Encryption (OME) coverage: Azure RMS activation as the required first step before sensitivity labels/mail flow rules, and SCEP as an unrelated device-certificate distractor.

---

## 2026-09-17

**Added**

- [[Microsoft Defender for Identity]] — sensor prerequisites (DSA/gMSA, Npcap, sizing, licensing, connectivity), the four actual sensor-host roles (DC/AD FS/AD CS/Entra Connect — not a generic server, not RADIUS), sensor v2.x vs. v3.x, detection categories, honeytoken accounts, and identity security posture assessments, pulled out of [[Securing Active Directory Domain Services (AD DS)]] into its own deep-dive note.
- [[Azure Blueprints]] — its deprecation, why (Azure Policy already owned enforcement), and the migration mapping to Template Specs (template packaging) + Deployment Stacks (deny-settings, lifecycle tracking) + Azure Policy (assignment).
- [[eDiscovery]] — the current eDiscovery/Premium eDiscovery model in the Microsoft Purview portal, the classic Content Search/Standard/Premium retirement (2025-08-31, except 21Vianet China), and the full terminology shift (Collections→Statistics, manual→automatic Advanced Indexing, Custodian-centric→Case-centric, Jobs→Processes).
- [[Microsoft Entra Built-in Roles]] — least-privilege role selection methodology for tricky, similarly-named Entra roles, the Secure Score read/write role list (Security/Exchange/SharePoint Administrator), and two fully worked exam scenarios.

**Updated**

- [[Securing Active Directory Domain Services (AD DS)]], [[Microsoft Defender]], [[AdminSDHolder and SDProp]], [[Microsoft Defender XDR]], [[Microsoft 365 Licensing]] — cross-linked to [[Microsoft Defender for Identity]].
- [[Azure Policy]], [[Microsoft Cloud Security Benchmark (MCSB)]], [[Azure Landing Zones]] — cross-linked to [[Azure Blueprints]] and corrected stale "Azure Policy/Blueprints" enforcement wording now that Blueprints is deprecated.
- [[Intune]] — added software update coverage (update rings, Windows Autopatch, driver/firmware updates) and when Configuration Manager (SUP/WSUS, Distribution Points) or Azure Update Manager (Windows Server) is still the right answer instead.
- [[DevOps Security]] — added the full DevSecOps lifecycle model (Plan → Develop → Build → Deploy → Operate) with an AKS task-to-stage mapping table, sourced from Microsoft's AKS-specific DevSecOps architecture guide.
- [[Container and Kubernetes Security]] — cross-linked to the DevSecOps lifecycle table in [[DevOps Security]].
- [[PIM]] — full rewrite: terminology table, the active-vs-eligible distinction (and why active needs zero activation steps), the complete activation/assignment-duration/active-creation role-settings breakdown, the Entra-role vs. Azure-resource-role permission split, PIM for Groups, and the full deployment plan.
- [[Purview]], [[Compliance and Privacy]] — cross-linked to [[eDiscovery]].
- [[Secure Score Mechanics]] — added the Secure Score permissions model (Defender Unified RBAC + the Entra global role read/write vs. read-only list) that explains why Exchange/SharePoint Administrator reach Secure Score.
- [[Security Posture Assessments]] — added Foundational CSPM vs. Defender CSPM plan comparison (including the October 27, 2026 opt-in cutover) and the AWS connector's OIDC federation authentication flow (audience/signature/certificate-thumbprint/role-condition validation, no stored AWS keys).
- [[services|Services]], [[Architecture Decisions]] — rows for all new notes and decision points from this push.

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
