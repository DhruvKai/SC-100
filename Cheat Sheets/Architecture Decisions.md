# Architecture Decisions

Decision-tree style quick reference. Update as new decision points come up.

```
Need SIEM?
  ↓
[[Microsoft Sentinel]]

Need endpoint protection (server, client/BYOD, or IoT/OT)?
  ↓
[[Securing Server and Client Endpoints]]

Need cloud posture?
  ↓
[[Microsoft Defender for Cloud]]

Need ransomware/BCDR resiliency strategy?
  ↓
[[Ransomware Resiliency and BCDR]]

Need to secure AI/Copilot adoption, or use AI to defend?
  ↓
[[AI and Copilot Security Architecture]]

Need to know where sensitive data is and who can reach it?
  ↓
[[Data Security Posture Management (DSPM)]]

Need to design the SOC itself (workspace topology, detection coverage)?
  ↓
[[Security Operations]]

Need to source/operationalize threat intelligence (IOCs, actors, TTPs)?
  ↓
[[Threat Intelligence]]

Need to replace VPN or apply identity-aware control to network traffic?
  ↓
[[Identity as the Security Perimeter]]

Need to scope what an identity/workload can actually do (RBAC, managed identity, tenancy)?
  ↓
[[Identity and Access Management (IAM)]]

Need to reduce standing privilege (JIT, bundled access, over-provisioned permissions)?
  ↓
[[Securing Privileged Access]]

Need runtime protection for a running VM/container/database, not just config scoring?
  ↓
[[Cloud Workload Protection (CWPP)]]

Need to prioritize findings across posture + workload + data + permissions signals?
  ↓
[[CSPM and CWPP]]

Need to secure a VM's network exposure or a PaaS resource's reachability?
  ↓
[[Securing IaaS and PaaS Services]]

Need to evaluate/design network topology, perimeter, or DDoS/WAF/firewall controls?
  ↓
[[Network Security Architecture]]

Need to identify design-time threats for a business-critical app (STRIDE)?
  ↓
[[Threat Modeling]]

Need to secure the pipeline itself (secrets, code, dependencies, IaC)?
  ↓
[[DevOps Security]]

Need to discover, classify, handle (DLP/retention), or encrypt organizational data?
  ↓
[[Data Classification and Protection]]

Need autonomous attack containment, verdict-based remediation, or cross-workload Defender admin delegation?
  ↓
[[Microsoft Defender XDR]]

Need to store/protect secrets, keys, or certificates (including HSM-backed)?
  ↓
[[Key Vault]]

Need to discover shadow SaaS usage or control an app's session in real time?
  ↓
[[SaaS Application Discovery and Control]]
```
