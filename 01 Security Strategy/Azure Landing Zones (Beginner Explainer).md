---
tags:
  - sc100
type: primer
domain:
  - best-practices
aliases:
  - Landing Zones Explained
  - Landing Zones for Beginners
---

# Azure Landing Zones — Beginner Explainer

> **What this note is**: a from-scratch, example-heavy teaching walkthrough of Azure Landing Zones — deliberately *not* written in this vault's usual terse revision-card style. Use [[Azure Landing Zones]] for exam-day quick revision once this has clicked; use this note the first time the concept doesn't make sense, or whenever it needs re-explaining from zero.

---

## The problem landing zones solve

Imagine a company — call them **Contoso** — decides to start using Azure. Day one, there's nothing: no structure, no rules, no shared services. What actually happens in most companies without a plan:

- The marketing team creates an Azure subscription and starts deploying a website.
- The finance team creates a *different* subscription and deploys a database.
- The HR team creates *another* subscription for an internal app.
- Each team names things differently, sets up security differently (or not at all), nobody's centrally watching the logs, nobody agreed on how networks connect to each other, and if someone in IT security asks "which of our 40 subscriptions have a publicly exposed database," nobody can answer quickly.

This is called **sprawl**, and it's the #1 reason organizations end up with security holes in the cloud — not because any single team did something obviously wrong, but because there was no shared foundation everyone built on top of.

**Azure Landing Zones are Microsoft's answer to "how do we let many teams deploy to Azure quickly, without everyone reinventing security, networking, and governance from scratch — and without losing central control."**

---

## The analogy: a business park

Picture a company that develops a big business/office park. Before any tenant moves in, the developer builds:

- The roads and gates into the park (**networking**)
- A security company that patrols the whole property and monitors cameras (**central security monitoring**)
- A shared ID badge system that works at every building (**identity**)
- Water, power, and utility hookups running to every building lot (**shared platform services**)
- Rules every tenant must follow — fire code, signage rules, etc. (**governance/policy**)

Once that's done, when a new tenant (say, a coffee shop) wants to move in, they don't need to build their own road, their own security force, or negotiate their own power connection. They just move into a **prepared lot** that already has all of that plumbed in, and they only have to worry about their own shop.

**That prepared lot is a landing zone.** The tenant "lands" into an environment that's already secure, already connected, already governed — they didn't have to build any of that themselves.

In Azure terms:
- The **developer's shared infrastructure** (roads, security, utilities) = the **platform landing zone** — built *once*, centrally managed.
- **Each tenant's individual shop** = an **application landing zone** — built *many times*, once per team/workload, and each one automatically inherits everything the platform provides.

Keep this picture in mind — it comes back constantly below.

---

## Prerequisite: how Azure organizes things

Azure has a nesting structure, like folders inside folders:

```
Tenant (your whole organization's identity boundary — this is Entra ID)
  └── Management Group (a folder for grouping subscriptions)
        └── Management Group (folders can nest inside folders)
              └── Subscription (a billing + resource boundary)
                    └── Resource Group (a folder inside a subscription)
                          └── Resource (an actual thing — a VM, a database, a storage account)
```

**Concrete example:**
- Contoso has one **tenant**: `contoso.onmicrosoft.com`. This is their whole company's identity — every user, every group, every login happens here.
- Inside that tenant, they create a **management group** called "Contoso" as the top folder.
- Inside that, a **subscription** called "Marketing-Prod" — this is where marketing's production resources live, and it's also a billing boundary (Contoso gets one Azure bill per subscription, broken down by resource).
- Inside that subscription, a **resource group** called "marketing-website-rg" — a logical folder holding everything for one website.
- Inside that resource group, the actual **resources**: a Virtual Machine, a Storage Account, a Web App.

**Why management groups matter so much:** you can assign a rule (an Azure Policy, an RBAC permission) at a management group level, and it automatically applies to *every subscription underneath it* — even subscriptions that don't exist yet when you create the rule. This is the single most important mechanic in the whole landing zone story, so here's a concrete example:

> Contoso's security team assigns a policy at the "Contoso" management group: **"Deny creation of any storage account with public internet access enabled."**
>
> Six months later, someone creates a brand-new subscription for a new project, nested under that same management group. The moment they try to create a storage account with public access turned on, **it gets blocked automatically** — even though nobody manually configured anything in that new subscription. The rule inherited down the tree.

This is exactly like the business park's fire code — it doesn't matter which building you move into, the fire code silently applies to you because you're inside the park's boundary.

---

## What a "landing zone" literally is

A landing zone is **not a single resource you deploy**. It's a whole *design pattern* — a specific structure of management groups, subscriptions, policies, networking, and identity, arranged in a standard way, that Microsoft recommends every organization use.

It has exactly **two flavors**:

### 1. Platform landing zone (built once)

This is Contoso's "business park infrastructure" — the shared stuff every workload needs, built centrally, by a platform team, **one time** for the whole organization. It's broken into (typically) four subscriptions, each with a specific job:

| Subscription | What actually lives here | Business-park equivalent |
|---|---|---|
| **Identity** | Domain controllers (if Contoso still runs on-prem Active Directory), Entra Connect servers (which sync on-prem users into the cloud), Recovery Services vaults for backing those up | The shared ID badge system |
| **Management** | A Log Analytics workspace (where every log from every subscription gets sent), [[Microsoft Sentinel]] (the SIEM watching all of it), dashboards leadership looks at | The security control room with all the camera feeds |
| **Connectivity** | The **hub VNet** (central virtual network), [[Azure Firewall]], DDoS Protection, DNS, VPN/ExpressRoute gateways connecting back to Contoso's physical offices | The roads, gates, and the connection back to the highway |
| **Security** | [[Microsoft Defender for Cloud]] configuration — the posture/protection layer watching every resource in every subscription | The security company's contract covering the whole park |

Notice: **none of these subscriptions run Contoso's actual business applications.** They're pure infrastructure — the stuff every application will *depend on*, but no app "lives" here.

### 2. Application landing zone (built many times)

This is one of Contoso's actual "shops" — an environment for **one specific workload or team**. Every time a new project needs an environment, a *new application landing zone* gets created. Concrete examples at Contoso:

- **"Marketing-Website" application landing zone** — hosts the public marketing site. One subscription (or more), with its own resource groups for the web app, its database, its storage.
- **"HR-Portal" application landing zone** — hosts an internal-only HR system.
- **"Finance-Reporting" application landing zone** — hosts finance's Power BI + data pipeline.

Each of these is a *separate subscription*, separately billed, separately owned by that team — but **all of them automatically inherit the guardrails the platform team set up**: they're already connected to the hub network, already sending logs to the central Sentinel workspace, already covered by Defender for Cloud, already subject to the org-wide policies (like "no public storage accounts"). The marketing team never had to configure any of that themselves — same as the coffee shop never had to build its own road.

---

## The management group tree, in real detail

Microsoft's reference design (and what you'll see in every Azure landing zone diagram) looks like this:

```
Tenant Root Group
 │
 ├── Platform                      ← the "business park infrastructure," one per org
 │    ├── Identity
 │    ├── Management
 │    ├── Connectivity
 │    └── Security
 │
 ├── Landing Zones                 ← where actual application landing zones live
 │    ├── Corp                     ← internal-only workloads
 │    │     ├── HR-Portal (an application landing zone / subscription)
 │    │     └── Finance-Reporting (another one)
 │    ├── Online                   ← internet-facing workloads
 │    │     └── Marketing-Website (another one)
 │    └── Local                    ← workloads with data residency/regional requirements
 │          └── Germany-Customer-Data (another one)
 │
 ├── Sandbox                       ← throwaway experimentation, isolated from prod
 │
 └── Decommissioned                ← subscriptions being retired, quarantined before deletion
```

Why **Corp vs. Online vs. Local** specifically?

- **Corp** = workloads only reachable from inside Contoso's own network (an internal HR tool nobody outside the company should ever touch). These get tighter network policies — no direct internet exposure.
- **Online** = workloads meant to be public on the internet (the marketing website). These get policies suited to internet-facing resources — e.g., a Web Application Firewall requirement.
- **Local** = workloads that must respect data residency rules (e.g., a subsidiary in Germany where customer data legally can't leave Germany). These get region-locking policies.

Each of those three is its own management group, so each can carry a **different set of inherited policies** suited to that category — a Corp app doesn't need the same public-facing hardening rules an Online app does, and applying those rules to internal-only apps would just be noise.

**This is exactly why the structure is a *tree* and not a flat list** — every branch can carry different rules, and everything under that branch inherits them automatically, the same way the storage account policy example worked earlier.

---

## Subscription vending — the "moving-in" process

Here's the problem subscription vending solves: without it, when the HR team needs a new subscription, someone in the platform team has to *manually*:

1. Create the subscription
2. Move it under the right management group (Corp, in this case)
3. Manually assign 15+ policies
4. Manually connect it to the hub network
5. Manually set up RBAC so the right people have the right access
6. Manually connect logging to the central Sentinel workspace

That's slow, error-prone, and doesn't scale past a handful of teams.

**Subscription vending is an automated, self-service pipeline that does all six of those steps for you.** Concretely, it usually looks like: someone fills out a short form (or triggers a pipeline) saying "I need a new landing zone for the HR team, category = Corp," and within minutes, automation spins up a subscription that's *already* peered to the hub network, *already* has the right policies attached (inherited automatically from being placed under the Corp management group), *already* has baseline RBAC configured.

Back to the business park analogy: subscription vending is the **leasing office**. The new tenant fills out paperwork, and the property manager's process automatically hooks up their unit to water, power, security, and the road — the tenant doesn't lay a single brick themselves.

---

## "Do we build this ourselves?" — Accelerators

Designing this whole tree, all the policies, the network topology, from a blank page would take a platform team months. Microsoft publishes a **Landing Zone Accelerator** — pre-built Infrastructure-as-Code (Bicep or Terraform templates) that deploys this *entire* structure for you: the management group tree, the baseline policies, the hub network, the four platform subscriptions — all wired together, following Microsoft's best practices out of the box.

Using the accelerator is like buying a prefab building kit instead of hiring an architect to design a house from scratch — faster, tested, and you only deviate from the standard design where you have a genuinely unusual requirement.

---

## Networking: how the hub connects to everyone

Every application landing zone needs to reach the shared services in the platform's Connectivity subscription (the firewall, DNS, the on-prem connection). Two ways to wire that up:

- **Hub & Spoke** — literally a wheel: one central "hub" VNet (in the Connectivity subscription) with each application landing zone's VNet "peered" (directly connected) to it, like spokes. You manage the hub yourself. Good for a moderate number of regions/spokes.
- **Virtual WAN** — Microsoft runs the "hub" for you as a managed global backbone; your spokes just plug into it. Less manual VNet peering management, scales much better when Contoso has, say, 20 regional offices and dozens of spokes across many regions — Microsoft's backbone handles the routing complexity instead of you.

Business park analogy: Hub & Spoke is the developer building and maintaining their own road network. Virtual WAN is like the business park being built right next to a highway interchange that a highway authority (Microsoft) already maintains — you just build your on-ramp.

---

## Putting it all together: Contoso's full journey

1. Contoso decides to move to Azure seriously. Before any app gets built, the platform team stands up **one platform landing zone**: Identity, Management, Connectivity, and Security subscriptions, using Microsoft's accelerator, with a Hub & Spoke network (Contoso only has 2 regions, so Virtual WAN would be overkill).
2. They set baseline policies at the top of the tree — e.g., "every resource must be tagged with a cost center," "no public storage accounts," "all VMs must report to the central Log Analytics workspace." These apply to *everything* that will ever be created under the tree, automatically.
3. The marketing team needs an environment for a new public website. Someone requests a landing zone via the subscription-vending process, tags it "Online." Minutes later they have a subscription that's peered to the hub, already policy-compliant, already logging to Sentinel — they just start deploying their website.
4. The HR team needs an internal tool. Same process, tagged "Corp" instead — inherits a slightly different, tighter set of network policies suited to internal-only apps.
5. Six months later, security tightens a rule at the top of the tree (say, requiring encryption at rest everywhere). It automatically applies to marketing's subscription, HR's subscription, and every subscription created after that point too — no one has to go update each one by hand.
6. Only *after* this baseline exists does it make sense to run a [[Cloud Adoption Security Review (CASR)]] — you can't meaningfully assess "is this landing zone secure" before the landing zone itself exists.

---

## Why this matters specifically for SC-100 (not AZ-500)

AZ-500 taught you *the individual controls* — how to configure a single Azure Policy, a single NSG rule, a single Defender for Cloud setting. **Landing zones are a level above that** — SC-100 is testing whether you can design the *structure* those controls get deployed into, at the whole-organization level, before any individual control matters. The exam-relevant facts, distilled from everything above:

- **Exactly one platform landing zone per tenant** — never design a second one; that's a red flag if you see it in a scenario.
- **Many application landing zones** — one per workload/team, created repeatedly via subscription vending.
- Governance (policy, RBAC) is applied at the **management group** level so it **inherits down automatically** — this is the mechanism that makes "guardrails already applied" possible without manual per-subscription work.
- **Corp / Online / Local** split lets different policy sets apply to internal-only, internet-facing, and residency-constrained workloads respectively.
- **Hub & Spoke vs. Virtual WAN** is a scale/complexity decision, not a right-vs-wrong one.
- Prefer the **Microsoft accelerator** over a custom build unless there's a genuine reason the reference design doesn't fit.

For the condensed, exam-revision version of all of this — the comparison tables, exam tips, and keyword list — see [[Azure Landing Zones]].

---

## Related Services

- [[Azure Landing Zones]]
- [[Cloud Adoption Framework (CAF)]]
- [[Cloud Adoption Security Review (CASR)]]
- [[Azure Well-Architected Framework (WAF)]]
- [[Network Security Architecture]]
- [[Azure Policy]]
- [[Microsoft Sentinel]]
- [[Microsoft Defender for Cloud]]

---

## References

- [What is an Azure landing zone?](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/) — Microsoft Learn
- [Azure landing zone accelerator](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/landing-zone-deploy) — Microsoft Learn
- [[Exam Objectives]]
