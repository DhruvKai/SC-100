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
| Need IoT device authentication (X.509/SAS/TPM via IoT Hub/DPS), not just device discovery? | [[Securing Server and Client Endpoints]] |
| Need App Service-specific hardening (Easy Auth, deployment slots) beyond generic PaaS network controls? | [[Securing IaaS and PaaS Services]] |
