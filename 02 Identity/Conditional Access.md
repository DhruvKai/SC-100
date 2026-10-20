---
tags:
  - sc100
type: concept
domain:
  - ops-identity-compliance
---
# Conditional Access

## Purpose

Conditional Access is [[Entra ID]]'s policy engine that grants, blocks, or limits access at sign-in time based on identity, device, location, and risk signals.

---

## Why Architects Choose It

- Primary enforcement point for [[Zero Trust]]'s "verify explicitly" principle — every sign-in is evaluated against current signals, not a one-time network check.
- Combines signals (user/group, app, location, device compliance, sign-in risk) into a single access decision, replacing scattered per-app controls. Device compliance itself traces back to hardware attestation — see [[Trusted Platform Module (TPM)]] for the chain that produces the Compliant Device signal.
- Extends beyond interactive users to workload identities and continuously re-evaluates sessions, closing gaps that static, sign-in-only checks leave open.

---

## When to Use

- Enforcing MFA, compliant-device, or session controls for specific apps, users, or risk levels.
- Blocking legacy authentication that can't support modern signal evaluation.
- Gating high-impact directory actions (role changes, CA policy edits) behind step-up verification.
- Restricting or monitoring sign-ins from workload identities (service principals, managed identities).

---

## When NOT to Use

- As a replacement for network segmentation or resource-level authorization — CA governs sign-in, not what a resource permits after access is granted (that's RBAC).
- As the only control for on-premises-only authentication paths that never reach Entra ID.
- Without an excluded break-glass emergency access account — a global CA misconfiguration can otherwise lock out all admins.

---

## Architecture

```mermaid
flowchart LR
    S["Signals:<br/>user/group, app, location,<br/>device compliance, sign-in risk"] --> E["Policy Engine"]
    R["Risk score<br/>(Identity Protection)"] --> E
    E --> D{Decision}
    D -->|Grant| G["Allow, optionally require<br/>MFA / compliant device"]
    D -->|Session| SC["Limit session:<br/>reauth frequency, app-enforced restrictions"]
    D -->|Block| B["Deny access"]
```

Multiple applicable policies combine with **AND** logic — a sign-in must satisfy every matching policy, not just one.

---

## Architecture Decisions

```mermaid
flowchart TD
    Q1["Gating a high-impact directory action<br/>(role assignment, CA policy edit)?"] -->|Yes| PA["Use Protected Actions"]
    Q1 -->|No| Q2["Securing a service principal or<br/>managed identity sign-in?"]
    Q2 -->|Yes| WI["Conditional Access for workload identities"]
    Q2 -->|No| Q3["Need to react mid-session to revoked<br/>access or elevated risk?"]
    Q3 -->|Yes| CAE["Continuous Access Evaluation"]
    Q3 -->|No| STD["Standard user-assignment CA policy"]
```

- Roll out new/changed policies in **Report-only** mode first; validate impact before enforcing.
- Always exclude a break-glass account from blocking policies.

### Who a policy can target

| Can target | Can't target |
| --- | --- |
| All users, specific users, groups (assigned/dynamic, nested) | [[Administrative Units\|Administrative units]] |
| **Built-in** directory roles | **Custom roles** |
| Guest/external user types (B2B, service provider…) | Roles scoped to an **administrative unit** or to a specific object |
| Single-tenant workload identities (service principals), agent identities (preview) | Managed identities; multitenant/third-party SaaS service principals |

---

## Exam Scenario — MFA for privileged accounts

**Scenario:** Entra ID **P1**. Evaluate **sign-on signals** during authentication. Initial testing phase: users get access, **elevated-privilege accounts require MFA**. Pick two tasks.

| Option | Verdict | Why |
| --- | --- | --- |
| **Deploy a CA policy from a template** | ✅ | Template **Require multifactor authentication for admins** |
| **Specify an assignment that includes built-in roles** | ✅ | CA Directory roles = **built-in roles only** |
| Add privileged accounts to an administrative unit | ❌ | AUs scope delegated administration. CA can't target them. See [[Administrative Units]]. |
| Define a custom role for administrators | ❌ | Custom roles aren't supported in CA assignments |
| Create App Control policies | ❌ | Device app allowlisting, not sign-in ([[App Control for Business and AppLocker]]) |

---

## Comparison

| Compare | Difference |
| --- | --- |
| Conditional Access vs. [[Identity Protection]] | Identity Protection *detects and scores* sign-in/user risk; Conditional Access *consumes* that score to grant, block, or challenge. |
| Conditional Access vs. RBAC | Conditional Access decides *whether* a sign-in is allowed; RBAC decides *what* an already-signed-in identity can do. |
| Conditional Access vs. [[PIM]] | Conditional Access evaluates every sign-in in real time; PIM governs standing vs. just-in-time privilege duration, independent of sign-in signals. |

---

## AZ-500 Review

AZ-500 already covers building individual policies: assignments (users, cloud apps, conditions), grant controls (require MFA, require compliant device), and session controls. That configuration-level knowledge is assumed here.

---

## What's New for SC-100

- Design Conditional Access as part of an org-wide [[Zero Trust]] strategy, not as a single isolated policy — includes validating existing policies for Zero Trust alignment (an explicit exam objective).
- Extend Conditional Access to **workload identities** (service principals, managed identities) — not just interactive users.
- Extend further to **agent identities** (Microsoft Entra Agent ID) for autonomous AI agents — see [[AI and Copilot Security Architecture]] for how this fits the broader AI security picture.
- Use **Continuous Access Evaluation (CAE)** to react to risk/revocation mid-session instead of waiting for token expiry.
- Use **Protected Actions** to require step-up authentication before high-impact directory changes, including changes to CA itself.
- Combine Conditional Access with Microsoft Entra Internet/Private Access (Global Secure Access) to add network-level compliance as a signal — full architecture in [[Identity as the Security Perimeter]].

---

## Exam Tips

- Break-glass accounts must always be excluded — expect distractor answers that "fix" an outage by loosening MFA instead.
- Report-only mode is the expected first step for any new policy recommendation, not immediate enforcement.
- Policies combine with AND — a scenario showing unexpected blocks is often caused by an additional policy the question didn't highlight.
- "Require MFA for admins" → CA template + **built-in directory roles** assignment. Distractors: administrative unit, custom role.
- "Evaluates sign-on signals as part of the authentication decision" = Conditional Access (needs **P1**; risk-based conditions need P2).

---

## Common Exam Confusion

- **Conditional Access vs. Identity Protection** — one enforces, the other detects; questions often test whether you can identify which system produced a signal vs. which acted on it.
- **Conditional Access vs. PIM** — tested by scenarios mixing "block risky sign-in" (CA) with "limit standing admin access" (PIM); pick based on whether the control is about the sign-in event or the privilege duration.
- **Conditional Access vs. [[Administrative Units]]** — CA is a sign-in policy engine. AUs scope *which objects an admin can manage*. AUs can't be a CA target.

| Target admins in CA via… | Supported? |
| --- | --- |
| Built-in directory role (e.g. Global Administrator) | ✅ |
| Group containing admins | ✅ |
| Custom role | ❌ |
| Administrative unit / AU-scoped role | ❌ |

---

## Keywords

- Policy engine, sign-in-time evaluation
- Grant controls vs. session controls
- Continuous Access Evaluation (CAE)
- Protected Actions
- Conditional Access for workload identities
- Break-glass / emergency access account
- Report-only mode
- Policies combine with AND logic
- CA templates, "Require multifactor authentication for admins"
- Directory roles assignment: built-in roles only

---

## Related Services

- [[Entra ID]] — Conditional Access requires P1; risk-based policies require P2.

- [[Entra ID]]
- [[Identity Protection]]
- [[PIM]]
- [[Zero Trust]]
- [[AI and Copilot Security Architecture]]
- [[Identity as the Security Perimeter]]
- [[Identity and Access Management (IAM)]]
- [[Trusted Platform Module (TPM)]]
- [[Administrative Units]] — can't be targeted by CA
- [[Microsoft Entra Built-in Roles]] — the only role type CA can target
- [[App Control for Business and AppLocker]] — distractor in the admin-MFA scenario

---

## References

- [Conditional Access documentation](https://learn.microsoft.com/en-us/entra/identity/conditional-access/) — Microsoft Learn
- [[Exam Objectives]]
