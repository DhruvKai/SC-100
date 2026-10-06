---
tags:
  - sc100
type: concept
domain:
  - infrastructure
aliases:
  - Web Protection
  - Web Threat Protection
  - Web Content Filtering
  - Network Protection
  - Custom Indicators
  - MDE Web Protection
status: needs-verification
---

# Defender for Endpoint Web Protection

## Purpose

Defender for Endpoint's browser-focused protection (**web threat protection + web content filtering + custom indicators**). It blocks phishing, exploit and malware sites on the device itself, wherever the device is connected.

---

## Why architects choose it

- **Enforced on the endpoint, not the network.** It protects remote/roaming laptops off-VPN without a proxy or gateway.
- **Browser coverage:** Microsoft Edge natively via SmartScreen; **Chrome, Firefox** and other browsers via **network protection** under the hood.
- Category-based **web content filtering** and URL/domain **custom indicators** are managed in one place, with reporting in [[Microsoft Defender XDR]].
- Uses MDE licences already purchased, so no extra infrastructure.

---

## When to use

- "Protect users from web-based threats / phishing / exploit sites" on **remote** devices.
- "Protection for **Edge, Firefox and Chrome**" → web protection (not a browser-specific feature).
- Block whole website categories (adult, high-bandwidth, leisure) → web content filtering.

---

## When NOT to use

- Blocking a few specific URLs/IPs/files/certificates only → **custom indicators** (a component, not the whole solution).
- Protecting server-to-server or Azure network traffic → [[Azure Firewall]] / [[Network Security Architecture]].
- SaaS app discovery/control (shadow IT) → Defender for Cloud Apps ([[SaaS Application Discovery and Control]]).
- Wanting a central proxy-based SWG for all traffic → Entra Internet Access ([[Identity as the Security Perimeter]]).

---

## Components

| Component | What it does |
| --- | --- |
| **Web threat protection** | Blocks phishing, exploit hosting, malware, untrusted/low-reputation sites |
| **Web content filtering** | Blocks by **category** of website |
| **Custom indicators** | Admin-defined allow/block for specific **URLs/domains, IPs, files (hashes), certificates** |
| **Network protection** (dependency) | OS-level, blocks outbound connections to malicious domains/IPs from **any process**. It extends SmartScreen beyond Edge and powers web protection in third-party browsers |

---

## Exam Scenario

**Scenario:** Remote-worker laptops fail an Azure security benchmark audit, and MDE licences have been bought. Requirements: protect users from web-based threats; cover **Edge, Firefox and Chrome**; block exploit and phishing sites; protect devices **wherever connected**.

| Option | Verdict | Why |
| --- | --- | --- |
| **Deploy web protection** | ✅ | Browser-focused protection, builds on network protection, includes URL/category filtering, works on any network |
| Enable network protection | ❌ | System-wide domain/IP blocking but **not browser-specific** web threat/category filtering on its own |
| Create custom indicators | ❌ | Only blocks admin-specified URLs/IPs/files/certs, not comprehensive web threat protection |
| Configure a proxy server | ❌ | MDE does **not rely on a proxy**; protection runs on the endpoint |

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Web protection vs. network protection** | Web protection = the browser-threat + category-filtering feature set (the "solution" answer). Network protection = the OS-level enforcement layer it builds on (domain/IP blocking for all processes). |
| **Web protection vs. custom indicators** | Comprehensive reputation + category protection vs. a manual list of specific IoCs. |
| **Web protection vs. proxy / SWG** | On-device enforcement anywhere vs. traffic routed through a gateway. |
| **SmartScreen vs. network protection** | SmartScreen = Edge (and Windows) reputation checks. Network protection extends that blocking to Chrome/Firefox and other apps. |
| **MDE web content filtering vs. Defender for Cloud Apps** | Category blocking of websites vs. sanctioning/unsanctioning **cloud apps** (shadow IT). |

---

## Exam Tips

- "Edge, Chrome **and** Firefox" + "phishing/exploit sites" + "wherever connected" → **web protection**.
- "Block a specific URL/hash/cert" → **custom indicators**.
- Proxy is always wrong for MDE web protection.
- Network protection is part of the plumbing but alone is "insufficient" in this question pattern.

---

## AZ-500 overlap

- AZ-500 has little or no MDE endpoint web filtering; it focuses on Azure resources.
- Known: the concept of IoCs, URL filtering and firewalls.

---

## What's new in SC-100

- Designing endpoint security baselines for **remote/hybrid** workers without a network perimeter ([[Zero Trust]]).
- Picking the device-enforced control over network-centric controls when users roam.
- Mapping audit findings against benchmarks ([[Microsoft Cloud Security Benchmark (MCSB)]]) to specific endpoint features.

---

## Keywords

- Web protection, web threat protection, web content filtering
- Network protection, SmartScreen
- Custom indicators: URLs/domains, IPs, file hashes, certificates
- Phishing sites, exploit sites, malicious domains
- Edge, Chrome, Firefox; remote workers; wherever connected
- No proxy required

---

## Related Services

- [[Securing Server and Client Endpoints]]
- [[Microsoft Defender XDR]]
- [[Microsoft Defender]]
- [[Intune]]
- [[Identity as the Security Perimeter]]
- [[SaaS Application Discovery and Control]]
- [[Zero Trust]]
- [[Microsoft Cloud Security Benchmark (MCSB)]]
- [[App Control for Business and AppLocker]]

---

## References

- [Protect your organization against web threats](https://learn.microsoft.com/en-us/defender-endpoint/web-threat-protection) — Microsoft Learn
- [Web protection](https://learn.microsoft.com/en-us/defender-endpoint/web-protection-overview) — Microsoft Learn
- [Protect your network (network protection)](https://learn.microsoft.com/en-us/defender-endpoint/network-protection) — Microsoft Learn
- [Create indicators](https://learn.microsoft.com/en-us/defender-endpoint/manage-indicators) — Microsoft Learn

---

## Verification Flag

- The browser-coverage detail (Edge via SmartScreen; third-party browsers via network protection) is from training-knowledge recall. Re-check the web protection overview for current OS/browser support (including macOS/Linux).
