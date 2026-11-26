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
| Integrating (legacy) Update Management with ConfigMgr, updates not from ConfigMgr and no WSUS? | Give the Windows agent **Microsoft Update** access — [[Azure Update Manager]] (full tree under Decision Trees below) |
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
| Need backups replicated across availability zones with data kept in the same region? | **ZRS** on the Recovery Services vault — [[Azure Storage Redundancy]] (GRS/GZRS leave the region) |
| Need to restore backups in the paired region during a regional disaster? | **GRS + Cross Region Restore** — [[Azure Storage Redundancy]] |
| Which CAF security discipline covers a dev team adopting DevOps? | **Innovation security** — [[Cloud Adoption Framework (CAF)]] |
| Website traffic surge, keep it available (WAF)? | **Scale out** (horizontal), not scale up — [[Azure Well-Architected Framework (WAF)]] |
| Sentinel: correlate low-impact events (exploit chaining) / turn Defender for Cloud Apps alerts into incidents / catch after-hours logins? | **Fusion / Microsoft Security / Anomaly** — [[Sentinel Analytics Rule Types]] |
| Categorize Azure Monitor data by source (app perf, guest OS/containers, tenant, REST)? | **Application / Infrastructure / Azure Platform / Custom** — [[Azure Security Logging]] |
| Match OAuth 2.0 roles to processes? | Owner **authorizes**, client **requests**, authorization server **gives scoped access**, resource server **accepts tokens** — [[OAuth 2.0 and OpenID Connect]] |
| Evaluate an existing subscription against ISO 27001? | **Add the standard** in Defender for Cloud — [[Assigning Regulatory Compliance Standards]] |
| Continuous export at resource-group level, to Logic Apps, or cross-tenant? | **Defender for Cloud REST API** — [[Defender for Cloud REST API]] |
| Which MCSB control is "protect identity and authentication systems"? | **IM-2**: restrict privileged roles, strong auth for privileged access, audit high-risk activity — [[Microsoft Cloud Security Benchmark (MCSB)]] |
| Deploy Microsoft's updated security baseline across endpoints? | **Intune security baselines** (not OSA, a framework) — [[Intune]] |
| BitLocker on Entra-joined Windows 11 VDI VMs, no agent, OS-level? | **Intune disk encryption policy** (not ConfigMgr, SSE or Defender for Storage) — [[Intune]] |
| IoT Hub firewall ports? | **MQTT 8883, AMQP 5671, anything over WebSockets/HTTPS 443** — [[IoT Device Authentication and Transport Security]] |
| Manage client data on mobile devices? | **Conditional Access + app protection policies** (not UEBA) — [[Securing Server and Client Endpoints]] |
| Moving APIs to Azure Functions: what do you get automatically? | **No infrastructure to manage + pay per execution.** Not HSM secrets (Key Vault), not hardened containers, not CDN — [[Azure Functions]] |
| Which Azure Firewall rule type: internet → private server / server → partner custom port / server → FQDN over HTTPS? | **DNAT + network / network / application** — [[Azure Firewall]] |
| Expose one VM on 80/443 only, future VMs private? | **Public IP on that NIC + NSG inbound web rules** (NAT Gateway = outbound only; firewall app rules don't do inbound) — [[Network Security Group]] |
| Defender for Cloud Apps: block download on unmanaged device / risk report from firewall logs / impossible travel? | **CA App Control / Cloud Discovery / Anomaly detection policy** — [[SaaS Application Discovery and Control]] |
| Company must manage keys M365 uses for data at rest: first step after prerequisites? | **Run the Customer Key Onboarding Service**, then DEPs — [[Microsoft 365 Customer Key]] |
| Protect SSH/RDP access to server VMs? | **Azure Bastion** (JIT only shrinks the exposure window; NSG = filter; forced tunneling = egress) — [[Azure Bastion]] |
| Evaluate and remediate Defender for Cloud alerts with workflow automation, minimal development? | **Logic Apps** (not Functions = code, Event Hubs = streaming, Azure Monitor webhooks = HTTP callback) — [[Defender for Cloud Workflow Automation]] |
| Stream Azure/Defender security data to Splunk/QRadar/third-party SIEM? | **Event Hubs** (diagnostic settings / continuous export) — [[Azure Event Hubs]] |
| Event Hubs vs. Event Grid vs. Service Bus? | **Stream telemetry / route discrete events / transactional messages** — [[Azure Event Hubs]] |
| Azure Monitor alert must call an Entra-authenticated endpoint? | **Secure webhook** action in the action group — [[Azure Monitor Alerts and Action Groups]] |
| Run PowerShell remediation on on-prem servers from Azure? | **Azure Automation + Hybrid Runbook Worker** — [[Azure Automation]] |
| Sentinel: visualize / automate response / Python-ML hunting? | **Workbook / playbook / notebook** — [[Sentinel Workbooks, Playbooks and Notebooks]] |

---

## Decision Trees

Multi-step decisions that don't fit in one table row.

### Patching / update source

```
What are you patching?
  ↓
Windows 10/11 CLIENTS            → Intune update rings / Windows Autopatch
SERVERS (Azure/on-prem/AWS/GCP,
  Windows + Linux)               → Azure Update Manager (+ Azure Arc for non-Azure)
Legacy Windows estate,
  disconnected/low-bandwidth     → Configuration Manager (SUP = WSUS + Distribution Points)
Third-party apps via AUM         → publish them to WSUS

Every Windows machine needs ONE update source:
  ↓
Gets updates from ConfigMgr?     → ConfigMgr SUP (WSUS underneath) is the source
NOT from ConfigMgr + WSUS exists → point the Windows agent at WSUS
NOT from ConfigMgr + no WSUS     → give the Windows agent Microsoft Update access
```

**Legacy question: "Integrate Update Management with ConfigMgr. What's the prerequisite?"**

```
Tick off what the stem already gives you:
  ✔ Update Management added to the Automation account
  ✔ Log Analytics workspace with Update Management enabled
  ✔ ConfigMgr present (current branch 1606+)
  ↓
The one prerequisite left = the Windows agent's update source
  ↓
"Security updates are NOT received from ConfigMgr" + no WSUS mentioned
  ↓
Give the Windows agent access to Microsoft Update ✅
```

| Distractor | Why it's wrong |
| --- | --- |
| Configure ConfigMgr to manage security updates | The stem states "not from ConfigMgr" as a **fact**, not a problem to fix. The goal is to let Update Management do it. |
| Configure the agent to talk to WSUS | No WSUS server exists. Building one isn't a prerequisite, it's a new deployment. |
| Hybrid-join servers and devices | An [[Intune]]/[[Conditional Access]] requirement, not a patching one. |

See [[Azure Update Manager]], [[Microsoft Configuration Manager]], [[WSUS]], [[Security Updates and Patch Management]].

### Backup / storage redundancy

```
Must data stay in ONE region (data residency)?
  ├─ YES → survive a zone/datacenter failure?
  │          ├─ YES → ZRS   ← "replicate across availability zones, same region"
  │          └─ NO  → LRS   (cheapest; rack/drive failures only)
  └─ NO  → survive a REGIONAL outage?
             ├─ restore backups in the paired region → GRS + Cross Region Restore
             ├─ storage account, zones + region      → GZRS (not offered for Recovery Services vaults)
             └─ read the secondary without failover  → RA-GRS / RA-GZRS
```

Recovery Services vault: **default GRS**, options **LRS/ZRS/GRS**, set **before** the first backup. Zones = ZRS, regions = GRS. → [[Azure Storage Redundancy]]

### CAF security disciplines

```
DevOps / DevSecOps / dev team / pace of innovation  → Innovation security
Boundaries, segmentation, network + identity        → Access control
Detect / respond / recover, SOC                     → Security operations
Protect data/apps/infra, brownfield + greenfield    → Asset protection
Posture over time, compliance, delegated decisions  → Security governance
```

Match the discipline to **what's being changed**, not to who's asking. → [[Cloud Adoption Framework (CAF)]]

### Scaling for availability (WAF)

```
Traffic surge + "maintain availability"?
  → SCALE OUT (more instances + load balancer + autoscale)
Can't split the workload (stateful, singleton)?
  → scale up as a stop-gap, then redesign
Slow for distant users (latency)?
  → Front Door / CDN (performance, not capacity)
Async backend work bursts?
  → Queue-based load leveling
```

Availability = **more** instances, not **bigger** ones. → [[Azure Well-Architected Framework (WAF)]]

```
IaaS web estate overwhelming minimal staff + autoscale + Azure DevOps?
  → Azure App Service (PaaS); not VMSS (still IaaS overhead), not Functions (event code)
+ DDoS protection for those websites/REST APIs, low effort?
  → Azure CDN (inherent DDoS at the edge) / Front Door (+ WAF if OWASP is named)
```

Every compute option autoscales. **Minimal admin overhead** decides it. → [[Azure App Service]], [[Azure CDN]]

```
Breach via unpatched OS → move APIs to serverless (Azure Functions):
  ✔ No infrastructure/OS to manage (Microsoft patches)
  ✔ Pay only while code runs (Consumption / Flex)
  ✘ Secrets in HSMs        → only if YOU add Key Vault (Premium) / Managed HSM
  ✘ Hardened containers    → a container platform, not plain Functions
  ✘ Assets on a CDN        → a separate Azure CDN / Front Door decision
```

If the statement needs **another service added on purpose**, it's not an impact of the move. → [[Azure Functions]]

### Sentinel analytics rule type

```
Many weak signals that only matter together (exploit chaining) → Fusion
Alerts already raised by another Microsoft product             → Microsoft Security rule
Unusual for this user (after hours, odd volume)                → Anomaly
Anomalous RDP/SSH login                                        → ML Behavior Analytics
Own KQL logic, minutes matter                                  → NRT (every minute)
Own KQL logic, otherwise                                       → Scheduled
Logs vs. Microsoft TI indicators                               → Threat Intelligence matching analytics
```

In the Defender portal, Fusion and Microsoft Security rules are **off**, and Defender XDR creates and correlates those incidents. → [[Sentinel Analytics Rule Types]]

### Security automation engine

```
Which product raised the event?
  Defender for Cloud alert / recommendation / compliance → Workflow automation → Logic App
  Sentinel incident / alert / entity                     → Automation rule → playbook (Logic App)
  Defender XDR endpoint / identity / email               → Native AIR / attack disruption (no build)
  Azure Monitor metric / log / Activity Log alert        → Action group (email, webhook, Logic App,
                                                           Function, runbook, Event Hub, ITSM)

What must happen?
  Act with minimal development (connectors, approvals)   → Logic Apps
  Custom code / heavy parsing                            → Azure Functions (often called by a Logic App)
  PowerShell on hybrid/on-prem machines                  → Azure Automation (Hybrid Runbook Worker)
  Move the data to another SIEM / data lake              → Event Hubs (continuous export / diagnostic settings)
  Just notify an existing HTTP endpoint                  → Webhook (secure webhook if Entra auth needed)
```

→ [[Defender for Cloud Workflow Automation]], [[Playbooks and Automation Rules]], [[Logic Apps]], [[Azure Event Hubs]], [[Azure Monitor Alerts and Action Groups]]

### Sentinel workbooks, playbooks, notebooks

```
Show it (dashboard, report, trend, compliance view) → Workbook (Azure Monitor Workbooks, KQL)
Do something (notify, enrich, remediate)            → Playbook (Logic App) via automation rule
Analyze in code (Python, ML, external data)         → Notebook (Jupyter + MSTICPy on Azure ML)
  …at data-lake scale, scheduled                    → Data lake notebooks / jobs (VS Code, PySpark)
Saved KQL hunt                                      → Hunting query (not a notebook)
```

→ [[Sentinel Workbooks, Playbooks and Notebooks]]

### Azure Monitor data source category

```
No install needed (resources, Activity log, Entra tenant logs) → Azure Platform
Agent in guest OS / container cluster                          → Infrastructure
SDK in the app code (performance + functionality)              → Application
You push it via REST API                                       → Custom sources
```

→ [[Azure Security Logging]]

### Compliance evaluation and export (Defender for Cloud)

```
Evaluate EXISTING resources against a NAMED standard (ISO/PCI/NIST)?
  → Add the standard (Regulatory compliance → Manage compliance policies)
Microsoft's own baseline / Secure Score?
  → MCSB (= Azure Security Benchmark): already ON by default
Unlock non-default standards?
  → Paid Defender plan (= "enhanced security features"): prerequisite only
Deploy a NEW environment to a compliant pattern?
  → Template Specs + Deployment Stacks + Azure Policy (Blueprints = deprecated)

Continuous export…
  RG scope / Logic Apps / cross-tenant / multiple configs → REST API
  Same config on many subscriptions                        → Azure Policy (DeployIfNotExists)
  One subscription → Log Analytics / Event Hubs            → Azure portal
```

→ [[Assigning Regulatory Compliance Standards]], [[Defender for Cloud REST API]]

### Identity: OAuth roles, MCSB IM controls, admin MFA

```
OAuth 2.0 flow:  Client REQUESTS → Resource owner AUTHORIZES →
                 Authorization server GIVES SCOPED ACCESS (token) →
                 Resource server ACCEPTS requests with the token

MCSB IM:  IM-2 protect identity system  = restrict privileged roles + strong auth
                                          for privileged access + audit high-risk activity
          IM-3 app identities (managed identity)   IM-6 strong auth/passwordless for all
          IM-7 Conditional Access                  IM-8 secrets exposure

Require MFA for admins (P1)?
  → CA policy from template "Require MFA for admins" + assignment = BUILT-IN roles
     ✗ administrative units   ✗ custom roles   ✗ App Control for Business (device allowlisting)
```

→ [[OAuth 2.0 and OpenID Connect]], [[Microsoft Cloud Security Benchmark (MCSB)]], [[Conditional Access]]

### Azure Firewall rule type

```
1. Who STARTS the connection?
   OUTSIDE → IN (customers, contractors, partners reaching YOUR private resource)
     → DNAT rule (firewall public IP:port → private IP:port) + network rule
       restrict the source; inbound HTTP(S) needing L7 → WAF, not app rules
   INSIDE → OUT / east-west (your server reaching the internet, partner, other spoke)
     → go to 2
2. Is it HTTP / HTTPS / MSSQL identified by a DOMAIN (FQDN, URL, web category)?
     YES → Application rule   (FQDN tags, web categories; URL filtering = Premium + TLS inspection)
     NO  → Network rule       (IP + port + TCP/UDP/ICMP: RDP, SSH, SQL, SMTP, custom ports)
3. Block known-bad IPs/domains before any rule?
     → Threat intelligence filtering (evaluated first)
```

| Requirement | Rule |
| --- | --- |
| Customers → API endpoint (internet → private) | DNAT + network |
| Contractors → database server (internet → private) | DNAT + network |
| Transaction server → card processor on TCP 6868 | Network |
| Server → `process.payments.com` over HTTPS | Application |
| VMs → Windows Update only | Application (FQDN tag) |

Order: **TI → DNAT → Network → Application → infrastructure FQDNs → deny.** Rules are terminating, so a network rule match skips application rules. → [[Azure Firewall]]

### Microsoft 365 data encryption keys

```
Company must manage/revoke keys M365 uses for data AT REST?
  → Customer Key
     prereqs: licence → 2 Azure subs → service principals → Key Vaults (soft delete + purge
              protection) → create/import RSA keys (no expiry)
     FIRST after prereqs: Customer Key Onboarding Service (Validate → Enable)
     then: data encryption policies (multi-workload / Exchange mailbox / SharePoint-OneDrive)
Microsoft must be UNABLE to decrypt specific content?
  → Double Key Encryption (labels; breaks search/eDiscovery/Copilot for that content)
Protection that travels with the file/email?
  → Sensitivity labels (Azure RMS)
Control Microsoft engineer access?
  → Customer Lockbox
```

→ [[Microsoft 365 Customer Key]], [[Double Key Encryption (DKE)]]

### Defender for Cloud Apps components

```
Uploaded firewall/proxy logs → app risk report (shadow IT) → Cloud Discovery
Block download/copy in-session on unmanaged devices        → Conditional Access App Control
Impossible travel / infrequent country / mass download     → Anomaly detection policy
Risky OAuth app permissions                                → App Governance
"Data encryption policy"                                   → Customer Key, NOT Defender for Cloud Apps
```

→ [[SaaS Application Discovery and Control]]

### Inbound vs. outbound building blocks

```
INBOUND (internet → your VM/app)
  One VM, simple             → Public IP on NIC + NSG allow 80/443 (default DenyAllInBound)
  Many VMs, L4               → Public Load Balancer + NSG
  Web app needing L7/OWASP   → App Gateway / Front Door + WAF
  Through a central firewall → Azure Firewall DNAT (+ network rule)
  Admin RDP/SSH              → Azure Bastion (no public IP on the VM)
OUTBOUND (your VMs → internet)
  Plain SNAT, scale          → NAT Gateway (outbound ONLY, never inbound)
  Filter by FQDN/category    → Azure Firewall application rule
  Filter by IP/port          → Azure Firewall network rule / NSG outbound rule
GROUPING
  Rules by VM role           → ASG (only works inside an NSG rule)
```

→ [[Network Security Group]], [[Azure Firewall]]

### VM admin access (SSH/RDP)

```
Securely SSH/RDP into Azure VMs from anywhere, no public IP, no open 22/3389?
  → Azure Bastion   (Premium = session recording / private-only)
Shrink the exposure window on a direct path that still exists?
  → JIT VM access (Defender for Servers P2), a complement, not the access solution
Filter which IPs/ports reach a subnet?
  → NSG (building block)
Route/inspect OUTBOUND internet traffic?
  → Forced tunneling / Azure Firewall (unrelated to inbound admin access)
Network-level or per-app private access for many resources?
  → VPN Gateway / Entra Private Access
```

"Protecting SSH **access**" = Bastion. "Reduce **exposure** of open management ports" = JIT. → [[Azure Bastion]], [[Securing IaaS and PaaS Services]]

### Endpoints: baselines, BitLocker, mobile data, IoT ports

```
Deploy/update a security baseline across devices → Intune security baselines
  (OSA = framework, Autopilot = provisioning, Purview = data)
BitLocker, Entra-joined Windows VMs/VDI, no agent → Intune disk encryption policy
  (ConfigMgr needs its client agent; SSE = platform layer; Defender for Storage = threats)
Client data on mobile devices                    → Conditional Access + app protection policies
  (UEBA detects, it doesn't manage data)
IoT Hub ports                                    → MQTT 8883 · AMQP 5671 · WebSockets/HTTPS 443 · never 80
```

Intune isn't mobile-only. It manages Windows 10/11 including Entra-joined Azure VMs (VDI). → [[Intune]], [[Securing Server and Client Endpoints]], [[IoT Device Authentication and Transport Security]]
