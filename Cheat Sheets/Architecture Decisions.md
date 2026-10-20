---
tags:
  - sc100
type: cheat-sheet
---

# Architecture Decisions

Decision-tree style quick reference. Update as new decision points come up.

| Question | Answer |
| --- | --- |
| Need SIEM? | [[Microsoft Sentinel]] |
| Need endpoint protection (server, client/BYOD, or IoT/OT)? | [[Securing Server and Client Endpoints]] |
| Need cloud posture? | [[Microsoft Defender for Cloud]] |
| Need ransomware/BCDR resiliency strategy? | [[Ransomware Resiliency and BCDR]] |
| Need to secure AI/Copilot adoption, or use AI to defend? | [[AI and Copilot Security Architecture]] |
| Need faster SOC triage/investigation via natural-language AI, or a scoped autonomous SOC agent? | [[Microsoft Security Copilot]] |
| Need to know where sensitive data is and who can reach it? | [[Data Security Posture Management (DSPM)]] |
| Need to design the SOC itself (workspace topology, detection coverage)? | [[Security Operations]] |
| Need to source/operationalize threat intelligence (IOCs, actors, TTPs)? | [[Threat Intelligence]] |
| Need to model an attacker's lifecycle (comms narrative vs. SOC coverage mapping) or map a real incident to one? | [[Attack Chain Models]] |
| Need which framework applies at all (Zero Trust/CAF/WAF/MCSB/MCRA/SAF/SFI/CASR)? | [[Frameworks Cheat Sheet]] |
| Need the WAF Security pillar's 12-point checklist or its antipatterns/tradeoffs? | [[Azure Well-Architected Framework (WAF)]] |
| Need common Zero Trust rollout mistakes to sanity-check a design against? | [[Zero Trust]] |
| Need CAF's Govern methodology (policy MVP, Five Disciplines of Cloud Governance)? | [[Cloud Adoption Framework (CAF)]] |
| Need who owns what across on-prem/IaaS/PaaS/SaaS (or the AI-specific version)? | [[Shared Responsibility Model]] / [[AI and Copilot Security Architecture]] |
| Need to evaluate or secure an OT/ICS/SCADA network? | [[OT and ICS Security]] |
| Need to find internet-facing assets the org doesn't know it owns (shadow IT, subdomain takeover, post-M&A)? | [[External Attack Surface Management (EASM)]] |
| Need to replace VPN or apply identity-aware control to network traffic? | [[Identity as the Security Perimeter]] |
| Need to scope what an identity/workload can actually do (RBAC, managed identity, tenancy)? | [[Identity and Access Management (IAM)]] |
| Need to reduce standing privilege (JIT, bundled access, over-provisioned permissions)? | [[Securing Privileged Access]] |
| Need runtime protection for a running VM/container/database, not just config scoring? | [[Cloud Workload Protection (CWPP)]] |
| Need to prioritize findings across posture + workload + data + permissions signals, or explain Defender for Cloud as a CNAPP? | [[CSPM and CWPP]] |
| Need to secure a VM's network exposure or a PaaS resource's reachability? | [[Securing IaaS and PaaS Services]] |
| Need to evaluate/design network topology, perimeter, or DDoS/WAF/firewall controls? | [[Network Security Architecture]] |
| Need to identify design-time threats for a business-critical app (STRIDE)? | [[Threat Modeling]] |
| Need to secure the pipeline itself (secrets, code, dependencies, IaC)? | [[DevOps Security]] |
| Need to discover, classify, handle (DLP/retention), or encrypt organizational data? | [[Data Classification and Protection]] |
| Need to translate a named regulation into technical/organizational controls, or respond to litigation/legal hold? | [[Compliance and Privacy]] |
| Need to fulfill an individual's data access/erasure request, or detect privacy-specific risk (overexposure, cross-border transfer)? | [[Priva]] |
| Need autonomous attack containment, verdict-based remediation, or cross-workload Defender admin delegation? | [[Microsoft Defender XDR]] |
| Need to store/protect secrets, keys, or certificates (including HSM-backed)? | [[Key Vault]] |
| Need to discover shadow SaaS usage or control an app's session in real time? | [[SaaS Application Discovery and Control]] |
| Need to know which Entra ID license a control requires, or activate a role JIT? | [[Entra ID]] / [[PIM]] |
| Need co-management, zero-touch device provisioning, or JIT local admin elevation? | [[Intune]] |
| Need NSG rule mechanics (service tags, ASGs) or Azure Firewall rule mechanics (NAT/network/application)? | [[Network Security Group]] / [[Azure Firewall]] |
| Need a global or regional L7 reverse proxy, or the right DDoS Protection plan? | [[Front Door and Application Gateway]] / [[DDoS Protection]] |
| Need to enforce/deny configuration or understand what drives Secure Score? | [[Azure Policy]] |
| Need to prove a device's boot chain wasn't tampered with, or back BitLocker/Windows Hello/Credential Guard with hardware? | [[Trusted Platform Module (TPM)]] |
| Need to publish/secure an API (throttling, token validation, versioning) in front of a backend? | [[API Management and Security]] |
| Need to secure a container image, an AKS cluster, or pod-to-Azure authentication? | [[Container and Kubernetes Security]] |
| Need Windows Server/AD DS admin tasks delegated without full Domain Admin rights? | [[Securing Privileged Access]] (JEA) |
| Need to detect and score sign-in/user risk for Conditional Access to consume? | [[Identity Protection]] |
| Need browser/native-client RDP-SSH with no public IP on the VM? | [[Azure Bastion]] |
| Need to verify an NSG rule, troubleshoot connectivity, or delegate management across tenants? | [[Network Watcher and Lighthouse]] |
| Need to secure Exchange/SharePoint/Teams mail and collaboration threats, or M365 Secure Score? | [[Securing Microsoft 365]] |
| Need IoT device authentication (X.509/SAS/TPM via IoT Hub/DPS), not just device discovery? | [[IoT Device Authentication and Transport Security]] |
| Need App Service-specific hardening (Easy Auth, deployment slots) beyond generic PaaS network controls? | [[Securing IaaS and PaaS Services]] |
| Need to know which Microsoft 365 SKU or add-on a required control needs (E3 vs. E5 vs. E5 Security/Compliance)? | [[Microsoft 365 Licensing]] |
| Need to stop a single compromised backup admin from deleting backups or disabling soft delete? | [[Resource Guard]] (Multi-User Authorization) |
| Need external incident response expertise, a compromise assessment, or an IR retainer? | [[Microsoft Incident Response (DART)]] |
| Need to know where to *start* with Zero Trust — a prioritized backlog with named owners? | [[Rapid Modernization Plan (RaMP)]] |
| Need to automate SOC response — and to pick between an automation rule, a playbook, and native XDR response? | [[Playbooks and Automation Rules]] |
| Need to specify security requirements for on-prem AD DS or reduce its attack surface? | [[Securing Active Directory Domain Services (AD DS)]] |
| Need to explain how a Secure Score is calculated, or turn a score target into a remediation plan? | [[Secure Score Mechanics]] |
| Need to back up on-prem/hybrid Windows files or domain-controller system state straight to Azure with no backup server? | [[MARS Agent]] |
| Need application-consistent backup of on-prem SQL/Exchange/SharePoint/VMs, or to protect Linux? | [[Microsoft Azure Backup Server (MABS)]] / DPM (not [[MARS Agent]]) |
| Need to place an account/device/role in the right containment boundary (Control/Management/Data-Workload plane, or a device security level)? | [[Privileged Access Tier Models]] |
| Need to explain why a protected account's ACL keeps reverting, or find orphaned former-admin accounts? | [[AdminSDHolder and SDProp]] |
| Need to turn a named regulation into a scored, tracked compliance posture, or decide where to assign/group an assessment? | [[Purview Compliance Manager]] |
| Need SQL-specific data protection — key-revocable at-rest encryption, tamper-evident history, or mandatory in-transit encryption? | [[SQL Data Protection (TDE, Ledger, TDS 8.0)]] |
| Need content even Microsoft can't decrypt? | [[Double Key Encryption (DKE)]] |
| Need to know exactly where/how to turn on a regulatory standard in Defender for Cloud, or how to actually get compliant? | [[Assigning Regulatory Compliance Standards]] |
| Need to automate Defender for Cloud (enable plans, assign standards, pull Secure Score) across many subscriptions via code? | [[Defender for Cloud REST API]] |
| Need to detect Kerberoasting, DCSync, golden ticket, or other on-prem AD attacks? | [[Microsoft Defender for Identity]] |
| Need to package/deploy RBAC + Policy + templates together, or migrate away from Azure Blueprints? | [[Azure Blueprints]] (deprecated — see its migration mapping) |
| Need Windows client update management, or to know when Configuration Manager/Azure Update Manager is still required? | [[Intune]] (client updates) / [[Azure Update Manager]] (server OS) / [[Microsoft Configuration Manager]] (legacy estate) |
| Need to map an AKS/DevOps task to the correct DevSecOps lifecycle stage (Plan/Develop/Build/Deploy/Operate)? | [[DevOps Security]] |
| Need to know whether an active or eligible PIM assignment requires activation, or which settings apply at activation vs. assignment creation? | [[PIM]] |
| Need to search, hold, or export content for litigation/investigation, or know that classic eDiscovery Standard/Premium is retired? | [[eDiscovery]] |
| Need to pick the least-privilege Entra role for a Secure Score / Purview / Exchange-alias task without over- or under-scoping? | [[Microsoft Entra Built-in Roles]] |
| Need to know which roles have read/write vs. read-only access to Microsoft Secure Score? | [[Secure Score Mechanics]] |
| Need to onboard an AWS/GCP account to Defender for Cloud, or know the CSPM free-vs-paid tier split? | [[Security Posture Assessments]] |
| Need to patch servers across Azure, on-prem, and AWS/GCP with one tool (Windows + Linux)? | [[Azure Update Manager]] + [[Azure Arc]] — strategy in [[Security Updates and Patch Management]] |
| Need an internal, approval-gated Windows update source, or third-party patches for Azure Update Manager? | [[WSUS]] |
| Integrating (legacy) Update Management with ConfigMgr, updates not from ConfigMgr and no WSUS? | Give the Windows agent **Microsoft Update** access — [[Azure Update Manager]] |
| Need to know how a posture weakness could be exploited (MITRE ATT&CK tactics/techniques)? | Open the [[Defender for Cloud Recommendations\|Defender for Cloud recommendation]] — not ARG, Quick fix, or the initiative |
| Need to query Defender for Cloud findings across many subscriptions? | [[Azure Resource Graph]] |
| Need data protected while in use — with code changes allowed (smallest TCB)? | SGX enclaves — [[Confidential Computing Enclaves]] |
| Need data protected while in use — lift-and-shift, no code changes? | Confidential VMs (AMD SEV-SNP / Intel TDX) — [[Confidential Computing Enclaves]] |
| Need OWASP Top 10 / SQLi / XSS / bot / geo-blocking protection for a web app? | [[Azure Web Application Firewall]] (Front Door = global edge, App Gateway = regional VNet) |
| Need to authenticate and govern autonomous AI agents at scale? | [[Microsoft Entra Agent ID]] (blueprints + Conditional Access for agents) |
| Need a map of everything Defender for Cloud does and which note covers it? | [[Microsoft Defender for Cloud]] |
| Need robust IoT device authentication + secure transmission for new devices in public/untrusted locations? | X.509 certificates + TLS 1.2 — [[IoT Device Authentication and Transport Security]] (field gateway only for legacy devices) |
| Need TTPs of a specific APT/threat actor from a Defender for Cloud alert? | Activity Group Report — [[Defender for Cloud Threat Intelligence Reports]] |
| Need to share CTI between TIPs using industry-standard formats/protocols with Sentinel? | STIX/TAXII connector (API root + Collection ID) — [[STIX and TAXII Connectors]] |
| Need a TIP/custom app without a TAXII server to push TI into Sentinel? | Upload API (legacy TIP connector = app ID + tenant ID + secret, deprecated) — [[Threat Intelligence Platform (TIP) Connector]] |
| Need to filter noisy TI feeds, link IOCs to actors, or export TI back to partners? | Ingestion rules / relationship builder / TAXII 2.1 export — [[Threat Intelligence Management in Sentinel]] |
| Need to delegate Entra admin rights over only one region/division's users? | [[Administrative Units]] (scoped role assignment) |
| Need to stop even Global Admins modifying executive accounts or sensitive groups? | Restricted management AU — [[Administrative Units]] |
| Need MFA for privileged accounts based on sign-in signals? | [[Conditional Access]] template + **built-in** directory roles (not AUs, not custom roles) |
| Need to passively monitor traffic between VMs for detection? | [[Virtual Network TAP]] |
| Need DDoS protection + faster load times for websites/REST APIs with minimal staff? | [[Azure CDN]] (or Front Door) |
| Need web hosting that autoscales, integrates with Azure DevOps, minimal admin overhead? | [[Azure App Service]] |
| Need to protect remote laptops from phishing/exploit sites in Edge, Chrome and Firefox? | [[Defender for Endpoint Web Protection]] |
| Need low-overhead testing to catch code vulnerabilities after changes? | Fuzzing + security regression testing — [[Application Security Testing]] |
| Need unified governance of on-prem, Kubernetes, AWS and GCP resources in place? | [[Azure Arc]] — Azure Policy on non-Azure resources + GitOps across clusters |
| Need to embed security across every SDLC phase for a new app? | [[Full Lifecycle Application Security Strategy]] (start with documented security requirements) |
| Need encrypted email that Gmail/Yahoo!/Outlook.com recipients can read? | [[Microsoft Purview Message Encryption]] — **first** verify Azure RMS is active |
| Need to encrypt/redirect/disclaim email automatically by condition? | [[Mail Flow Rules]] |
| Need to classify and protect files, emails, meetings or Teams/sites? | [[Sensitivity Labels]] (create, then publish via label policy) |
| Need device/user certificates from on-prem AD CS on Intune devices for Wi-Fi/VPN? | [[SCEP Certificate Infrastructure]] (NDES + Certificate Connector) |
| Need to prevent data exfiltration from an analytics workspace? | Managed VNet + data exfiltration protection — [[Azure Synapse Analytics]] |
