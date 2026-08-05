---
tags:
  - sc100
---

# Securing Server and Client Endpoints

## Purpose

Architecting endpoint protection across the full device estate — servers, client/mobile devices, and enterprise IoT/OT — matching each endpoint type to the right management and protection mechanism rather than one uniform control.

---

## Why Architects Choose It

- Endpoint types have fundamentally different risk profiles and management surfaces: a server, a BYOD phone, and a factory-floor sensor each need a different combination of identity, agent, and management tooling — one Zero Trust "Endpoints" pillar, several concrete implementations.
- Local admin credential reuse across machines is a classic lateral-movement vector — **Windows LAPS** closes it by rotating and uniquely randomizing local admin passwords per device, backed to Entra ID or AD DS.
- BYOD and unmanaged personal devices can't always accept full device management — **MAM** (app-level control) extends data protection to devices MDM can't fully enroll.
- IoT/OT devices often can't run an agent at all — Defender for IoT's passive network-sensor architecture protects what can't be instrumented directly.

---

## MDM and MAM

MDM and MAM are the two Intune-based answers to "how do we manage a client device," and the exam cares far more about *which one a scenario calls for* than how either is configured — that config-level detail is MD-102/Intune-admin territory, not SC-100.

- **Mobile Device Management (MDM)** — full device enrollment. The org gets compliance policies, configuration profiles, and remote/full wipe over the *entire device*. This is the answer for corporate-owned hardware, where full control is expected and acceptable.
- **Mobile Application Management (MAM)** — app-level control without enrolling the device. Intune **app protection policies (APP)** wrap specific managed apps (Outlook, Teams, etc.) with corporate data controls — require a PIN to open the app, block copy/paste into unmanaged personal apps, and **selective wipe**: removing only corporate data from the app on demand, leaving personal photos, messages, and apps untouched.
- **MAM without enrollment (MAM-WE)** is the BYOD architecture answer specifically: the *device* is never MDM-enrolled — only a lightweight Entra ID device registration happens (via a broker app, Microsoft Authenticator on iOS or Company Portal on Android) so Conditional Access can evaluate it. If a device later becomes MDM-managed, MAM-WE settings stop applying — the two aren't meant to stack in that direction.
- **MDM + MAM together** is normal for corporate-owned devices: full device compliance (MDM) *and* app protection policies (MAM) layered on top — defense in depth at two different levels, not an either/or choice in that scenario.
- **Why this matters architecturally**: MAM is the concrete, least-privilege application of Zero Trust to endpoints — it protects exactly the corporate data at risk (inside managed apps) instead of claiming authority over an entire personal device, which is both a privacy overreach and often legally/organizationally unacceptable for BYOD.
- **Conditional Access integration** — the **"Require app protection policy"** grant control is the mechanism that ties MAM into sign-in policy, the same way "Require compliant device" ties in MDM. Microsoft is migrating the older combined "Require approved client app or app protection policy" grant to a straight "Require app protection policy" requirement (cutover March 2026) — know the current grant control name, not the retired combined one.

---

## When to Use

- Protecting Azure-hosted or hybrid/multicloud servers with runtime detection — Defender for Servers (full depth in [[Cloud Workload Protection (CWPP)]]; this note covers where it fits in the broader endpoint picture).
- Enrolling and enforcing configuration on org-owned client devices — **Intune MDM** (full device management, compliance policies feeding [[Conditional Access]]).
- Protecting corporate data on unenrolled/BYOD devices — **Intune MAM**/app protection policies (app-level control, no full device enrollment).
- Rotating and randomizing local administrator passwords across a fleet — **Windows LAPS**, backed to Entra ID (cloud-native) or AD DS.
- Discovering and monitoring enterprise IoT devices (printers, cameras, badge readers) — **Defender for IoT Enterprise** edition, surfaced in Defender XDR.
- Monitoring true industrial control systems/OT networks — **Defender for IoT OT** edition, passive network sensors.
- Establishing a consistent starting configuration across a device fleet — Intune security baselines (client/mobile), alongside MCSB-based Azure baselines for servers (see [[Security Posture Assessments]]).

---

## When NOT to Use

- Assuming MDM enrollment is possible or desirable for every device — personal/BYOD devices often need MAM instead of a forced full-device policy.
- Deploying Defender for IoT's OT edition where Enterprise IoT would do — true ICS/plant-floor sensors are a different edition and deployment model than enterprise IoT device discovery.
- Treating Windows LAPS as a replacement for privileged access governance — it rotates *local* admin passwords; standing directory-level privilege still needs [[PIM]]/[[Securing Privileged Access]].

---

## Architecture

```mermaid
flowchart TD
    subgraph Endpoints["Endpoint types"]
        Servers["Servers<br/>(Azure/hybrid/multicloud)"]
        Clients["Client/mobile devices"]
        IoTEnt["Enterprise IoT<br/>(printers, cameras, badge readers)"]
        OT["OT/ICS<br/>(plant floor, substations)"]
    end

    Servers --> DfS["Defender for Servers<br/>(see CWPP)"]
    Clients --> Intune["Intune: MDM (org-owned)<br/>vs. MAM (BYOD app protection)"]
    Clients --> LAPS["Windows LAPS<br/>(local admin password rotation)"]
    IoTEnt --> DfIoTEnt["Defender for IoT — Enterprise<br/>(Defender for Endpoint add-on)"]
    OT --> DfIoTOT["Defender for IoT — OT<br/>(passive network sensors)"]

    DfS --> XDR["Defender XDR /<br/>Conditional Access signal"]
    Intune --> XDR
    DfIoTEnt --> XDR
    DfIoTOT --> XDR
```

---

## Architecture Decisions

```mermaid
flowchart TD
    Q1["Endpoint is a server?"] -->|Yes| A1["Defender for Servers (CWPP)"]
    Q1 -->|No| Q2["Org-owned device, full management acceptable?"]
    Q2 -->|Yes| A2["Intune MDM"]
    Q2 -->|No| Q3["BYOD/personal device needing<br/>corporate data protection only?"]
    Q3 -->|Yes| A3["Intune MAM / app protection policies"]
    Q3 -->|No| Q4["True industrial control / OT network?"]
    Q4 -->|Yes| A4["Defender for IoT — OT (passive sensors)"]
    Q4 -->|No| A5["Defender for IoT — Enterprise<br/>(printers, cameras, badge readers)"]
```

---

## Comparison

| Compare | Difference |
| --- | --- |
| MDM vs. MAM | MDM (Intune device enrollment) manages the whole device — compliance policies, wipe, full configuration. MAM (app protection policies) manages only corporate data inside specific apps, without enrolling the device — the BYOD answer when full enrollment isn't acceptable or possible. |
| Defender for IoT: Enterprise vs. OT edition | Enterprise IoT extends Defender for Endpoint to non-traditional enterprise devices (printers, cameras, badge readers) and surfaces them in Defender XDR; OT edition covers true industrial control systems via passive, agentless network sensors on plant-floor/substation networks. Different attack surface, different deployment model. |
| Windows LAPS vs. PIM | LAPS rotates and randomizes a *local* machine account's admin password per device; PIM governs *directory-level* standing privilege (Entra ID/Azure RBAC roles) — different layer, see [[Securing Privileged Access]] for the PIM side. A third layer, Endpoint Privilege Management (JIT elevation of a specific app/task without standing local admin), is covered in [[Intune]]. |
| Intune security baselines vs. MCSB | Intune baselines are pre-configured client/mobile device policy templates (Windows, Defender, Edge settings); MCSB is the cross-cloud resource-configuration benchmark used for servers/infrastructure (see [[Security Posture Assessments]]) — different scope, same "baseline-first" philosophy. |

---

## AZ-500 Review

AZ-500 doesn't cover Intune/endpoint management at all — MDM/MAM, Windows LAPS, and Defender for IoT are outside its Azure-resource-focused scope. AZ-500 does cover Defender for Servers-style workload protection at the resource level (already assumed in [[Cloud Workload Protection (CWPP)]]). Treat MDM/MAM, LAPS, and Defender for IoT as new territory for SC-100.

---

## What's New for SC-100

- Map endpoint type to the right mechanism as an explicit design exercise (server → CWPP, org-owned client → MDM, BYOD → MAM, enterprise IoT → Defender for IoT Enterprise, OT/ICS → Defender for IoT OT) rather than reaching for one "endpoint protection" answer.
- Know Windows LAPS as a specific, named, cloud-capable control (Entra ID- or AD DS-backed) — recently added to [[Microsoft Cybersecurity Reference Architectures (MCRA)|MCRA]] — and that Intune is the recommended management path for it.
- Distinguish Defender for IoT's Enterprise and OT editions by deployment model and target device type — a frequent exam trap given they share a product name.
- Feed device compliance and LAPS-backed posture into [[Conditional Access]] as a sign-in signal — endpoint security and identity architecture are one connected system, not separate concerns.

---

## Exam Tips

- "Protect corporate data on a personal, unenrolled phone" → Intune MAM/app protection policies, not MDM.
- "Remove company data from a departed employee's personal device without touching their photos" → MAM selective wipe, not a full MDM wipe.
- "Enforce app-level sign-in requirements without requiring full device enrollment" → the Conditional Access "Require app protection policy" grant control, not "Require compliant device."
- "Eliminate shared/reused local admin passwords across a Windows fleet" → Windows LAPS.
- "Discover and monitor badge readers, printers, and cameras" → Defender for IoT Enterprise edition, not the OT edition.
- "Monitor a plant-floor industrial control network without installing agents" → Defender for IoT OT edition, passive sensors.

---

## Common Exam Confusion

- **MDM vs. MAM** — full device management vs. app-level data protection; the BYOD signal points to MAM.
- **MAM-WE vs. MDM+MAM combined** — BYOD device never enrolled (only lightweight Entra ID registration) vs. corporate device fully enrolled *and* app-protected; the exam expects you to match the combination to device ownership, not assume they're interchangeable.
- **"Require compliant device" vs. "Require app protection policy"** — the MDM-backed Conditional Access signal vs. the MAM-backed one; a BYOD scenario needing the latter is a common distractor toward the former.
- **Defender for IoT Enterprise vs. OT** — enterprise device discovery (Defender for Endpoint add-on) vs. true industrial control monitoring (passive sensors); full comparison above.
- **Windows LAPS vs. PIM** — local machine credential rotation vs. directory-level standing privilege governance; see [[Intune]] for the third layer, Endpoint Privilege Management.

---

## Keywords

- Intune: Mobile Device Management (MDM) vs. Mobile Application Management (MAM)
- App protection policies (APP), selective wipe
- MAM without enrollment (MAM-WE), broker app (Authenticator/Company Portal)
- Conditional Access grant: "Require app protection policy" vs. "Require compliant device"
- BYOD, corporate-owned device
- Windows LAPS, local administrator password rotation
- Defender for IoT: Enterprise IoT edition vs. OT edition
- Passive network sensors, plant-floor/substation networks
- Security baselines (Intune baselines vs. MCSB)
- Device compliance signal (Conditional Access)

---

## Related Services

- [[Cloud Workload Protection (CWPP)]]
- [[Security Posture Assessments]]
- [[Conditional Access]]
- [[Securing Privileged Access]]
- [[Zero Trust]]
- [[Microsoft Defender]]
- [[Microsoft Cybersecurity Reference Architectures (MCRA)]]
- [[Intune]] — co-management, Autopilot, and Endpoint Privilege Management detailed there.

---

## References

- [Windows LAPS overview](https://learn.microsoft.com/en-us/windows-server/identity/laps/laps-overview) — Microsoft Learn
- [Enhance your OT security with Defender for IoT](https://learn.microsoft.com/en-us/azure/defender-for-iot/organizations/overview) — Microsoft Learn
- [IoT/OT security in Microsoft Defender XDR](https://learn.microsoft.com/en-us/defender-xdr/protect-against-iot-ot-threats) — Microsoft Learn
- [[Exam Objectives]]
