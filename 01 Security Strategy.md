---
tags:
  - sc100
type: index
---

# 01 Security Strategy

Frameworks, SOC design, resiliency and Microsoft's security programs. Covers exam domain **1 — Security best practices and priorities (20–25%)** and the SecOps part of domain 2. Back to [[00 Home]].

## Frameworks and Models

| Note | One line |
| --- | --- |
| [[Zero Trust]] | Verify explicitly, least privilege, assume breach: the lens for every design |
| [[Microsoft Cybersecurity Reference Architectures (MCRA)]] | Capability-to-Zero-Trust reference deck for gap analysis |
| [[Microsoft Cloud Security Benchmark (MCSB)]] | Cross-cloud control framework behind Defender for Cloud's default standard |
| [[Cloud Adoption Framework (CAF)]] | Seven-methodology adoption roadmap and when security is embedded |
| [[Azure Landing Zones]] | Multi-subscription platform + application environment from CAF Ready |
| [[Cloud Adoption Security Review (CASR)]] | Checklist review of an existing landing zone |
| [[Azure Well-Architected Framework (WAF)]] | Five-pillar per-workload review; Security pillar |
| [[Shift left (WAF)]] | Moving security earlier into the SDLC and pipeline |
| [[Security Adoption Framework (SAF)]] | Role-aware end-to-end security modernization model |
| [[Rapid Modernization Plan (RaMP)]] | Prioritized Zero Trust initiative checklists with owners |
| [[Secure Future Initiative (SFI)]] | Microsoft's own secure-by-design/default/operations commitment |
| [[Attack Chain Models]] | Cyber Kill Chain vs. MITRE ATT&CK |

## Security Operations

| Note | One line |
| --- | --- |
| [[Security Operations]] | SOC architecture: SIEM + XDR + SOAR, workspace topology, MITRE coverage |
| [[Microsoft Defender XDR]] | Cross-signal incident correlation, AIR, attack disruption |
| [[Playbooks and Automation Rules]] | Automation rules vs. playbooks vs. native XDR response |
| [[Sentinel Workbooks, Playbooks and Notebooks]] | Show (workbook) vs. act (playbook) vs. analyze (notebook) in Sentinel |
| [[Sentinel Analytics Rule Types]] | Scheduled, NRT, Microsoft Security, Anomaly, Fusion: which rule type for which detection |
| [[Threat Intelligence]] | Sourcing and operationalizing CTI (STIX/TAXII, TI in Sentinel) |
| [[STIX and TAXII Connectors]] | TAXII import (API root + Collection ID) and TAXII 2.1 export |
| [[Threat Intelligence Platform (TIP) Connector]] | Legacy Graph tiIndicators push (app ID + tenant ID + secret); Upload API |
| [[Threat Intelligence Management in Sentinel]] | Intel explorer, ingestion rules, relationships, ThreatIntelIndicators, export |
| [[Defender for Cloud Threat Intelligence Reports]] | Activity Group vs. Campaign vs. Threat Summary reports |
| [[Microsoft Security Copilot]] | Generative-AI SOC assistant and agents, SCU capacity |
| [[Microsoft Incident Response (DART)]] | Microsoft's external IR team and retainers |
| [[AI and Copilot Security Architecture]] | Securing AI workloads and using AI to defend |

## Resiliency and Updates

| Note | One line |
| --- | --- |
| [[Ransomware Resiliency and BCDR]] | Prepare / Limit / Prevent ransomware strategy, backup and recovery |
| [[Security Updates and Patch Management]] | Evaluating patching solutions: cloud-native vs. traditional |

## Service Pages

- [[Microsoft Sentinel]] · [[Microsoft Defender]] · [[Logic Apps]] · [[Azure Security Logging]]
- [[Azure Event Hubs]] · [[Azure Monitor Alerts and Action Groups]] · [[Azure Automation]]
- [[MARS Agent]] · [[Microsoft Azure Backup Server (MABS)]] · [[Resource Guard]] · [[Azure Storage Redundancy]]
- [[Azure Update Manager]] · [[WSUS]] · [[Microsoft Configuration Manager]]

See also: [[Exam Objectives]] · [[Architecture Decisions]] · [[Frameworks Cheat Sheet]]
