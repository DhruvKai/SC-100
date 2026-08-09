---
tags:
  - sc100
type: cheat-sheet
---
# Frameworks Cheat Sheet

Microsoft's overlapping strategy/architecture frameworks — one-line definitions + trigger keywords for fast recall. Full detail lives in each framework's own note; this page is the lookup table.

## Quick Reference Table

| Acronym | Full Name | One-liner | Recognize by (keywords) |
| --- | --- | --- | --- |
| [[Azure Well-Architected Framework (WAF)\|WAF]] | Azure Well-Architected Framework | Five pillars + self-assessment review evaluating **one workload's** architecture (Security is one of five pillars). | "Well-Architected Review", "five pillars", "per-workload", "trade-off analysis" |
| [[Cloud Adoption Framework (CAF)\|CAF]] | Cloud Adoption Framework | Seven-methodology roadmap sequencing **org-wide** adoption and ongoing governance/security. | "Strategy/Plan/Ready/Adopt/Govern/Secure/Manage", "[[Azure Landing Zones\|landing zone]]", "adoption lifecycle" |
| [[Cloud Adoption Security Review (CASR)\|CASR]] | Cloud Adoption Security Review | Checklist review scoring an **already baseline-secured** landing zone against CAF's Secure methodology. | "self-assessment vs. Microsoft-led", "CSAM", "baseline already met" |
| [[Microsoft Cloud Security Benchmark (MCSB)\|MCSB]] | Microsoft Cloud Security Benchmark | Scored, cross-cloud **control baseline**, pre-mapped to CIS/NIST/PCI-DSS; powers Secure Score. | "Domain / Control / Subcontrol", "pre-mapped to CIS/NIST/PCI-DSS", "Secure Score benchmark" |
| [[Microsoft Cybersecurity Reference Architectures (MCRA)\|MCRA]] | Microsoft Cybersecurity Reference Architectures | Downloadable deck mapping Microsoft (+ 3rd-party) **capabilities onto Zero Trust** — target-state / gap-analysis tool. | "capability reference architecture", "target-state architecture", "gap analysis", "CISO workshop" |
| [[Zero Trust]] | Zero Trust | Trust-verification **principle** (verify explicitly, least privilege, assume breach) used to evaluate any control. | "verify explicitly", "assume breach", "least privilege", "RaMP" |
| [[Security Adoption Framework (SAF)\|SAF]] | Security Adoption Framework (Microsoft security adoption model) | Umbrella, role-aware model — business scenarios → security disciplines → technology pillars — unifying MCRA, SDL, Zero Trust/CISO workshops, IR playbooks. | "business scenarios / security disciplines / technology pillars", "SAF workshops", "security adoption model" |
| [[Secure Future Initiative (SFI)\|SFI]] | Secure Future Initiative | Microsoft's own internal engineering commitment — six pillars mapped to Zero Trust + NIST CSF; a source MCSB synthesizes. | "Secure by Design/Default/Operations", "six pillars", "NIST CSF mapping" |

## Confusion Pairs

| Compare | Difference |
| --- | --- |
| WAF vs. CAF | WAF = one workload, five pillars. CAF = org-wide, seven methodologies. |
| CAF vs. CASR | CAF is the roadmap itself; CASR is the point-in-time review that scores a landing zone against CAF's Secure methodology. |
| MCSB vs. MCRA | MCSB = scored control baseline (compliance). MCRA = capability reference architecture (target-state design). Exam objective names them together — easy to conflate. |
| CASR vs. Well-Architected Review | CASR reviews landing zone security only; the Well-Architected Review evaluates one workload across all five WAF pillars. |
| WAF (framework) vs. [[Azure Firewall\|Azure Web Application Firewall]] | Identical acronym, unrelated: one is an architecture-evaluation framework, the other filters HTTP(S) traffic to web apps. |
| MCRA vs. Zero Trust | Zero Trust defines the principles; MCRA maps specific products onto those principles. |
| SAF vs. MCRA | SAF is the umbrella adoption model (business scenarios → security disciplines → technology pillars); MCRA is one integrated input into it, not a synonym. |
| SAF vs. CAF | SAF is product-/cloud-agnostic, end-to-end security modernization; CAF's Secure methodology is the security track specifically within Azure's adoption lifecycle. |
| SFI vs. MCSB | SFI is Microsoft's own internal engineering commitment (a source); MCSB is the customer-facing scored control baseline that partly synthesizes it. |

## Decision Shortcut

```
One workload's architecture trade-offs?
  ↓
WAF (Well-Architected Review)

Org-wide adoption sequencing / landing zone design?
  ↓
CAF

Landing zone already baseline-secure, need a maturity checkpoint?
  ↓
CASR

Need a scored, regulation-mapped control baseline?
  ↓
MCSB

Need a capability/target-state diagram or gap analysis?
  ↓
MCRA

Evaluating whether a design satisfies trust principles?
  ↓
Zero Trust

Need a role-aware, end-to-end security modernization roadmap (business → discipline → implementation)?
  ↓
SAF

Explaining why Microsoft enforces a secure default or deprecates a legacy protocol?
  ↓
SFI
```

## Keywords Index (fast lookup)

- "five pillars", "per-workload" → WAF
- "seven methodologies", "landing zone", "adoption lifecycle" → CAF
- "self-assess a landing zone", "already baseline-secure" → CASR
- "pre-mapped to CIS/NIST/PCI-DSS", "Domain/Control/Subcontrol" → MCSB
- "capability mapping", "target-state architecture", "gap analysis" → MCRA
- "verify explicitly", "assume breach", "least privilege" → Zero Trust
- "business scenarios / security disciplines / technology pillars", "SAF workshops" → SAF
- "Secure by Design/Default/Operations", "six pillars", "NIST CSF mapping" → SFI

## Related Services

- [[Cloud Adoption Framework (CAF)]]
- [[Azure Well-Architected Framework (WAF)]]
- [[Cloud Adoption Security Review (CASR)]]
- [[Microsoft Cloud Security Benchmark (MCSB)]]
- [[Microsoft Cybersecurity Reference Architectures (MCRA)]]
- [[Zero Trust]]
- [[Security Adoption Framework (SAF)]]
- [[Secure Future Initiative (SFI)]]
- [[Security Posture Assessments]]
- [[Azure Landing Zones]]
