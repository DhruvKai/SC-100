---
tags:
  - sc100
type: cheat-sheet
domain:
  - ops-identity-compliance
aliases:
  - Azure AD
  - Azure Active Directory
status: needs-verification
---

# Microsoft Entra ID

Microsoft's cloud identity platform — the IdP architecture (hybrid sync, external identities, decentralized identity) is covered in [[Identity as the Security Perimeter]]; this page is licensing and product-tier orientation, since a large share of this vault's recommended controls only exist at a specific tier. For how these tiers arrive bundled inside Microsoft 365 SKUs (P1 in E3, P2 in E5 or the E5 Security add-on), see [[Microsoft 365 Licensing]]. For hardening the on-prem directory that syncs into this one, see [[Securing Active Directory Domain Services (AD DS)]].

## License Tiers

- **Free** — included with any Azure/Microsoft 365 subscription: core directory (users, groups, on-prem sync via Entra Connect/Cloud Sync), basic SSO, self-service password *change* (not reset) for cloud-only accounts, and **Security Defaults** — a one-size-fits-all baseline MFA enforcement for the whole tenant, at no cost and with zero configuration.
- **P1** — adds [[Conditional Access]] (the granular, condition-based replacement for Security Defaults), hybrid identity features (self-service password reset with on-prem writeback), dynamic/self-service group membership and app assignment, and a 99.9% SLA.
- **P2** — adds everything in P1, plus **[[Identity Protection]]** (automated, continuous risk detection feeding Conditional Access risk-based policies) and **[[PIM|Privileged Identity Management]]** (JIT role activation) — the two capabilities this vault's privileged-access and risk-based sign-in architecture repeatedly assumes are available.
- **Microsoft Entra ID Governance** — a separately licensable add-on layered on top of *either* P1 or P2, bundling Entitlement Management, tenant-wide Access Reviews, Lifecycle Workflows, and Terms of Use (see [[Securing Privileged Access]] for the architecture role these play) — increasingly sold as its own SKU rather than assumed to come free with P2.

## Architecture

```mermaid
flowchart LR
    Free["Free<br/>(directory, Security Defaults)"] --> P1["P1<br/>+ Conditional Access, hybrid identity"]
    P1 --> P2["P2<br/>+ Identity Protection, PIM"]
    P1 -.add-on.-> Gov["Entra ID Governance<br/>(Entitlement Mgmt, Access Reviews,<br/>Lifecycle Workflows)"]
    P2 -.add-on.-> Gov
```

## Application Single Sign-On (SSO) Types

How an **enterprise application** (the per-tenant service principal side of an app registration — see [[Identity and Access Management (IAM)]]) is configured to authenticate through Entra ID, rather than prompting for standalone credentials. Not all five methods give the same security posture:

- **SAML-based SSO** — SAML 2.0 assertions; the method most gallery enterprise apps and custom SAML-aware apps use. Requires configuring an Identifier/Reply URL and a signing certificate.
- **OIDC/OAuth-based SSO** — modern token-based SSO, the flow behind app-registration-based (rather than pure gallery-SSO-config) applications and the API permission model covered in [[Identity and Access Management (IAM)]].
- **Password-based SSO** — Entra ID stores a shared username/password and replays it into the app's native web login form via a browser extension/agent. The **fallback of last resort** — no federation happens at all, the credential is stored and shared rather than eliminated. Only use when the app genuinely has no SAML/OIDC support.
- **Linked SSO** — the app already has SSO configured through a *different* IdP; Entra ID just links to it from the My Apps portal for discovery/launch — it doesn't broker authentication itself.
- **Disabled** — no SSO; the app prompts for its own credentials every time.
- **Header-based SSO** (via Azure AD Application Proxy, for published on-prem apps) — Application Proxy injects HTTP headers carrying user identity into apps that authenticate off trusted headers rather than a protocol handshake — the on-prem-publishing-specific case, mentioned as a Zero Trust Applications-pillar mechanism in [[Zero Trust]].

**Architecture decision** — federated SSO (SAML or OIDC/OAuth) is always the preferred answer: no shared credential, works with Conditional Access. Password-based SSO is a fallback for legacy apps only, not a default choice.

## Key Facts

- Security Defaults and Conditional Access are **mutually exclusive** — a tenant runs one or the other, not both; Conditional Access is the superset an org graduates to once it needs granular, condition-based control.
- Every Conditional Access pattern in this vault ([[Conditional Access]]) assumes at least **P1**; every pattern around risk-based sign-in or JIT privileged roles ([[PIM]], Identity Protection) assumes **P2**.
- Governance-scale capabilities (Entitlement Management, tenant-wide Access Reviews, Lifecycle Workflows) are licensed via the **Entra ID Governance add-on** — don't assume they're automatically included just because a tenant is licensed for P2.

## Exam Notes

- A scenario needing risk-based Conditional Access or JIT role activation, with no license named → the correct answer requires **P2**, not P1.
- A scenario satisfied by baseline, zero-config MFA for the whole tenant → **Security Defaults** (Free), not Conditional Access — a common over-engineering distractor.
- A scenario describing bundled, time-bound access packages or org-wide recertification campaigns → assume the **Entra ID Governance add-on**, not P2 alone.
- "App has no SAML/OIDC support at all" → password-based SSO is the only fit, not a security-preferred choice — a scenario implying it's the *default* answer is a trap.
- "App already federates with a different IdP, just needs to show up in My Apps" → linked SSO, not a new SSO configuration.

## Comparison

| Compare | Difference |
| --- | --- |
| Free vs. P1 | Free: baseline directory + Security Defaults MFA (all-or-nothing, zero config). P1: adds Conditional Access, hybrid identity (SSPR + writeback), dynamic groups — the tier this vault's Conditional Access architecture assumes throughout. |
| P1 vs. P2 | P2 adds Identity Protection (automated risk detection) and PIM (JIT privileged role activation) — the two capabilities [[Securing Privileged Access]] and risk-based Conditional Access design depend on. |
| P2 vs. Entra ID Governance add-on | P2 natively includes Identity Protection + PIM. Governance is a separate, additional add-on (purchasable on P1 or P2) for Entitlement Management, tenant-wide Access Reviews, and Lifecycle Workflows — don't conflate "has P2" with "has governance-scale entitlement/review capability." |
| Security Defaults vs. Conditional Access | Security Defaults: one-size-fits-all baseline MFA, Free tier, no configuration, mutually exclusive with CA. Conditional Access: granular, condition-based policy engine (P1) — full architecture in [[Conditional Access]]. |
| SAML/OIDC-based SSO vs. password-based SSO | SAML/OIDC federate authentication — no shared credential ever leaves the IdP. Password-based SSO stores and replays a shared username/password into the app's own login form — the credential still exists and is shared, just hidden from the user. Federated SSO is always preferred; password-based is the legacy-app fallback. |

## Related

- [[Conditional Access]]
- [[PIM]]
- [[Identity as the Security Perimeter]]
- [[Identity and Access Management (IAM)]]
- [[Securing Privileged Access]]
- [[Identity Protection]]
- [[Microsoft 365 Licensing]]
- [[Securing Active Directory Domain Services (AD DS)]]
- [[Exam Objectives]]

## References

- [Microsoft Entra ID pricing](https://www.microsoft.com/en-us/security/business/microsoft-entra-pricing) — Microsoft
- [What are security defaults?](https://learn.microsoft.com/en-us/entra/fundamentals/security-defaults) — Microsoft Learn
- [Microsoft Entra ID Governance](https://learn.microsoft.com/en-us/entra/id-governance/identity-governance-overview) — Microsoft Learn
- [What is single sign-on (SSO)?](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/what-is-single-sign-on) — Microsoft Learn

## Verification Flag

License-to-feature mapping — especially the Entra ID Governance add-on boundary — shifts periodically as Microsoft repackages SKUs. Re-verify exact tier/feature assignment against the current Microsoft Entra pricing page close to exam date.
