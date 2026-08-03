# Architecture Decisions

Decision-tree style quick reference. Update as new decision points come up.

```
Need SIEM?
  ↓
[[Microsoft Sentinel]]

Need endpoint protection?
  ↓
[[Defender XDR]]

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
```
