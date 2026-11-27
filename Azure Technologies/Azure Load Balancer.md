---
tags:
  - sc100
type: service
domain:
  - infrastructure
aliases:
  - Load Balancer
  - Cross-region Load Balancer
  - Gateway Load Balancer
---

# Azure Load Balancer

> **In one line:** a **Layer 4** (TCP/UDP) load balancer that sits **in the data path** and spreads traffic across VMs. It has no HTTP awareness, no WAF and no TLS termination.

## Purpose

Distributes TCP/UDP flows across a backend pool, either **regionally** (public or internal) or **globally** (cross-region tier).

---

## Key Facts

| Item | Detail |
| --- | --- |
| **Public vs. internal** | Public = internet-facing frontend IP. Internal = private frontend IP inside a VNet |
| **Standard SKU** | Current SKU: zone-redundant, 99.99% SLA, **secure by default** (inbound closed until an [[Network Security Group\|NSG]] allows it) |
| **Basic SKU** | Retired (30 Sept 2025): upgrade to Standard |
| **Gateway Load Balancer** | Transparently chains **third-party NVAs** (firewalls, IDS) into the traffic path of a Standard public LB or public IP |
| **Cross-region (global) tier** | Static **anycast** global IP in front of regional Standard LBs; instant failover, no DNS TTL |
| **HA ports** | Internal Standard LB rule for all ports/protocols: the pattern for **NVA high availability** |
| **Outbound** | Outbound rules give SNAT. **NAT Gateway** is the recommended outbound option (see [[Network Security Group]]) |
| **Health probes** | TCP / HTTP / HTTPS; unhealthy backends removed from rotation |

---

## Why Architects Choose It

- **Non-HTTP workloads** (SQL, RDP gateways, custom TCP/UDP, gaming) that need load balancing.
- **NVA high availability**: an internal LB with HA ports in a hub VNet, or Gateway LB for transparent insertion.
- Low latency, high throughput pass-through. Nothing is terminated or inspected.

## When to Use

- Regional TCP/UDP load balancing (public or internal).
- Active/active NVA or firewall appliance pairs.
- Global non-HTTP with instant failover → cross-region tier.

## When NOT to Use

- HTTP(S) needing WAF, TLS offload or path routing → [[Front Door and Application Gateway]].
- DNS-based global routing across Azure + on-prem → [[Azure Traffic Manager]].
- As a security control. Filtering is the NSG's job (Standard LB relies on it).

---

## Exam Tips

- **"Layer 4", "TCP/UDP", "regional", "non-HTTP"** → Load Balancer.
- **Standard LB blocks inbound by default.** No NSG allow rule = no traffic.
- **"Insert third-party firewall transparently"** → **Gateway Load Balancer**.
- **"Highly available NVAs in a hub"** → internal Standard LB + **HA ports**.
- **"Global, non-HTTP, instant failover, static IP"** → **cross-region** Load Balancer, not Traffic Manager.

## AZ-500 Overlap

- Known: NSGs, public vs. private IPs, NAT Gateway for outbound. SC-100 adds choosing LB vs. App Gateway vs. Front Door vs. Traffic Manager and the NVA HA pattern.

## What's New in SC-100

- Load balancing is an **availability** decision. Security comes from the NSG, firewall or WAF placed with it ([[Network Security Architecture]]).

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Load Balancer vs. Application Gateway** | L4 pass-through vs. L7 HTTP proxy with WAF/TLS offload |
| **Load Balancer vs. Traffic Manager** | In the data path, IP-based vs. DNS answer only, never sees traffic |
| **Cross-region LB vs. Traffic Manager** | Anycast IP, instant failover, Azure regional LBs only vs. DNS, TTL-delayed, any endpoint (incl. on-prem) |
| **Load Balancer vs. NAT Gateway** | Inbound + outbound load balancing vs. outbound-only SNAT |
| **Gateway LB vs. Azure Firewall** | Chains **your** third-party NVA vs. Microsoft's managed firewall |

## Keywords

- Layer 4, TCP/UDP, 5-tuple hash, backend pool, health probe, frontend IP
- Public / internal, Standard / Basic (retired), secure by default
- Gateway Load Balancer, NVA chaining, HA ports
- Cross-region (global) Load Balancer, anycast

## Related Services

- [[Azure Traffic Manager]] · [[Front Door and Application Gateway]] · [[Network Security Group]]
- [[Azure Firewall]] · [[Network Security Architecture]] · [[DDoS Protection]] · [[Architecture Decisions]]

## References

- [What is Azure Load Balancer?](https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-overview): Microsoft Learn
- [Gateway Load Balancer](https://learn.microsoft.com/en-us/azure/load-balancer/gateway-overview): Microsoft Learn
- [Cross-region load balancer](https://learn.microsoft.com/en-us/azure/load-balancer/cross-region-overview): Microsoft Learn
