---
tags:
  - sc100
type: concept
domain:
  - infrastructure
aliases:
  - Azure Content Delivery Network
  - Content Delivery Network
  - CDN
status: needs-verification
---

# Azure CDN

## Purpose

A global edge network that caches content close to users. Its large edge footprint also gives **inherent DDoS protection** to the origin (websites, REST APIs, media, files).

---

## Why architects choose it

- **DDoS absorption at the edge.** Attack traffic hits a huge distributed edge, not your origin VM or App Service.
- **Performance + availability** in one control: caching cuts origin load and latency. That answers "slow page loads cost revenue" and "attack overwhelmed web workloads" together.
- Fully managed, so there's nothing to patch or scale. That fits a "minimal IT staff" constraint.
- Origin-agnostic: it fronts IaaS, App Service, Storage or any public endpoint.

---

## When to use

- "Protect websites/REST APIs from DDoS" with low admin effort, plus latency/performance pain → **Azure CDN** (or [[Front Door and Application Gateway|Front Door]]).
- Static/cacheable content delivered globally.
- Offloading bandwidth from an origin.

---

## When NOT to use

- Need L7 **WAF** rules (OWASP, bots, rate limits) → [[Front Door and Application Gateway|Front Door Premium/Standard]] + [[Azure Web Application Firewall]].
- Need tuned **L3/L4** protection, an SLA, cost protection and Rapid Response for VNet public IPs → [[DDoS Protection]] (Network/IP Protection).
- Regional, VNet-internal, private-origin L7 routing → Application Gateway.
- New greenfield designs: Microsoft steers CDN workloads to **Azure Front Door Standard/Premium** (CDN + WAF + Private Link origin in one service).

---

## Exam Scenario

**Scenario:** Core REST APIs and websites are on Azure IaaS, IT staff is minimal, and an attack overwhelmed web workloads, causing latency and lost revenue. The requirements are: autoscale web workloads, DDoS protection for REST APIs/websites, and Azure DevOps integration.

| Requirement | Answer | Rejected |
| --- | --- | --- |
| Scaling + DevOps integration + minimal admin overhead | **[[Azure App Service]]** (PaaS, autoscale, Azure DevOps deployment) | Azure Functions (event-driven code, not hosting sites/APIs), Azure Spring Apps (Spring Boot microservices), VM Scale Sets (still IaaS to manage) |
| Protect workloads from DDoS | **Azure CDN** (DDoS protection inherent at the edge) | — |

Pattern: "minimize administrative overhead" pushes toward **PaaS**, so VMSS loses even though it autoscales.

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Azure CDN vs. Front Door** | CDN = caching + edge delivery. Front Door = global L7 load balancer + CDN + WAF + failover + Private Link origins (Premium). Front Door is the modern superset. |
| **Azure CDN vs. DDoS Protection** | CDN gets platform DDoS protection as a by-product of its edge. DDoS Network/IP Protection is a paid plan for **public IPs in a VNet**, with adaptive tuning, telemetry, SLA, cost protection and DRR. |
| **Azure CDN vs. WAF** | Volumetric absorption/caching vs. L7 request inspection (SQLi/XSS/bots). WAF attaches to Front Door/App Gateway. |
| **CDN vs. Traffic Manager** | Caches and serves content at the edge vs. DNS-based routing only (no caching, no proxy). |

---

## Exam Tips

- "DDoS for websites/REST APIs" + options without DDoS Protection plan/Front Door → **Azure CDN** (DDoS protection is inherent).
- "Slow load times" + "global users" → CDN/Front Door caching.
- If **WAF** or OWASP is named, choose Front Door + WAF, not plain CDN.

---

## AZ-500 overlap

- Known: DDoS Protection tiers, WAF on Front Door/App Gateway.
- New: CDN as an availability and DDoS architecture choice tied to business requirements (revenue, latency, staff).

---

## What's new in SC-100

- Treating availability (DDoS + performance) as a security requirement mapped to managed edge services.
- Picking the lowest-overhead managed service that satisfies the requirement, not the most configurable one.

---

## Keywords

- Content Delivery Network, edge, point of presence (POP), caching
- DDoS protection inherent, volumetric attack absorption
- Latency, slow page loads, global users
- Origin, static content, REST APIs, websites
- Front Door Standard/Premium (successor)

---

## Related Services

- [[Front Door and Application Gateway]]
- [[DDoS Protection]]
- [[Azure Web Application Firewall]]
- [[Azure App Service]]
- [[Network Security Architecture]]
- [[Securing IaaS and PaaS Services]]
- [[API Management and Security]]

---

## References

- [Azure security baseline for Content Delivery Network](https://learn.microsoft.com/en-us/security/benchmark/azure/baselines/content-delivery-network-security-baseline) — Microsoft Learn
- [What is a content delivery network on Azure?](https://learn.microsoft.com/en-us/azure/cdn/cdn-overview) — Microsoft Learn

---

## Verification Flag

- Azure CDN SKUs are being retired in stages. Akamai and Edgio are already retired, and Azure CDN Standard from Microsoft (classic) has an announced retirement with migration to Front Door Standard/Premium. Re-check current SKU status and dates before relying on "Azure CDN" as a current product name. The exam may still use it.
