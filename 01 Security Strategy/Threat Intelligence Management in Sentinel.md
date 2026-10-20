---
tags:
  - sc100
type: concept
domain:
  - ops-identity-compliance
aliases:
  - Intel explorer
  - Work with threat indicators
  - TI ingestion rules
  - ThreatIntelIndicators
status: needs-verification
---

# Threat Intelligence Management in Sentinel

## Purpose

How CTI is **created, curated, queried, visualized and exported** once it's in [[Microsoft Sentinel]]: Intel explorer, STIX objects, ingestion rules, relationships, tags, the `ThreatIntelIndicators` table, workbooks and TAXII export.

---

## Why architects choose it

- Raw feeds are noisy. **Ingestion rules** filter low-value IOCs and extend the life of high-value ones *before* they land, which cuts analyst noise and ingestion cost.
- **Relationships** (threat actor *uses* attack pattern, indicator *indicates* actor) turn flat IOC lists into attack context for response decisions.
- **TLP** marking makes sharing a governance decision before anything is exported.
- One table (`ThreatIntelIndicators`) feeds **analytics rules, hunting and workbooks**. That's the single detection dependency to design around.

---

## When to use

- Adding intel from your own investigations → **create STIX objects** manually.
- Feeds bring old/low-confidence noise, or trusted sources need longer validity → **ingestion rules**.
- Linking IOCs to actors/campaigns → **relationship builder** (not tags).
- Grouping objects for an incident → **tags**.
- Sharing curated intel with a partner/consortium → **Export** to a TAXII 2.1 server.

---

## When NOT to use

- Filtering intel from the **Upload API** or manually created objects with ingestion rules. Rules apply to **data connectors only**.
- Using tags to represent a known actor or campaign. Use a **relationship** instead.
- Expecting full edit rights on ingested objects. Only tags, expiration date, confidence and revoked are editable.
- Exporting to a TAXII 2.0 platform. Export is **TAXII 2.1 only**.

---

## Where it lives

| Portal | Path |
| --- | --- |
| **Defender portal** | **Threat intelligence → Intel explorer**. Combines TI objects **and Threat analytics reports** in one search experience. |
| Azure portal | Sentinel → **Threat management → Threat intelligence** (Azure portal Sentinel retires **March 31, 2027**) |

**Role:** **Microsoft Sentinel Contributor** or higher to manage TI. Import/export also requires the **Threat Intelligence solution** + relevant connectors.

---

## Create

- **Add new → TI object** → choose object type → fill required fields.
- Set **sensitivity / TLP** rating.
- Add **Relationship type + Target reference** at creation time if known.
- **Add and duplicate** copies common metadata to the next object for fast bulk entry.

**STIX objects available**

| Object | Represents |
| --- | --- |
| Threat actor | Who: motivation, sophistication, resourcing |
| Attack pattern | How (TTPs), mapped to a MITRE ATT&CK stage |
| Indicator | Domain, URL, IPv4/IPv6, file hashes, X509 certs, JA3/JA3S, user agents |
| Identity | Victims, orgs, sectors |
| Relationship | Links between objects |

**TLP**

| TLP | Share with |
| --- | --- |
| White | Anyone, publicly |
| Green | Community peers/partners, not public |
| Amber | Within the organization, not public |
| Red | Only the specific group/meeting where disclosed |

---

## Curate

### Ingestion rules (connector feeds only)

| Fact | Detail |
| --- | --- |
| Scope | **Data connectors only**. Not the Upload API, not manually created TI. |
| Actions | **Edit** (e.g. extend `Valid until`, add tag) or **Delete** (skip ingestion) |
| Order | Run from **lowest to highest order number**. Every object passes through each rule until a **Delete**. |
| Delete | Removes the object from the pipeline. **Previously ingested versions are unaffected.** |
| No match | Object ingested as-is |
| Latency | New/edited rules take **up to 15 minutes** |
| Side effect | Ingestion rules **don't update the object's `Modified` date** |

**Use cases from Learn**: drop low-confidence intel not updated for 6 months · extend `Valid until` +30 days for high-confidence IOCs from a trusted source · tag old threat actor names.

### Relationship builder

- **Add new → TI relationship**. **Max 20 relationships** in the builder at once. Build more over multiple iterations.

| Relationship | Pattern |
| --- | --- |
| Duplicate of / Derived from / Related to | Common, any STIX domain object |
| Targets | Attack pattern or Threat actor → Identity |
| Uses | Threat actor → Attack pattern |
| Attributed to | Threat actor → Identity |
| Indicates | Indicator → Attack pattern or Threat actor |
| Impersonates | Threat actor → Identity |

### Tags and edits

- Tags are **free-form**, so set **standard naming conventions**. Multi-select objects of the **same type** to tag in bulk.
- Edit **one object at a time**.
  - Created in Sentinel → **all fields** editable.
  - Ingested from TIP/TAXII → only **tags, Expiration date, Confidence, Revoked**.
- Only the **latest version** of an object is shown.

### Search and enrichment

- Intel explorer: keyword search, filter by type, **Targeted industries**, **Source**, attributes. No KQL needed. Changes can take a few minutes to appear.
- **GeoLocation + WhoIs** enrichment on IP and domain indicators (**preview**).

---

## Query, detect, visualize

| Item | Detail |
| --- | --- |
| Table | **`ThreatIntelIndicators`** (+ **`ThreatIntelObjects`** for non-indicator STIX objects) under the **Microsoft Sentinel** schema |
| Legacy table | `ThreatIntelligenceIndicator`. Ingestion stopped **July 31, 2025**, so update custom queries/rules/workbooks. |
| Consumers | **Analytics**, **Hunting**, **Workbooks** all query `ThreatIntelIndicators` |
| Where to query | Defender portal: **Advanced hunting**. Azure portal: **Logs**. |
| Lifecycle | Every create/update/delete = a **new row**. Deduplicated on `Id` (latest `TimeGenerated`). Sentinel **re-ingests all TI every 7–10 days**, billed at standard ingestion rates. |
| Detection | Built-in **TI analytics rule templates** per indicator type (domain, email, file hash, IP, URL). **Defender TI matching analytics** gives high-fidelity incidents. |
| Workbook | Built-in **Threat Intelligence** workbook (Azure Monitor workbooks). Save it before customizing. |

---

## Export

1. Configure a **TAXII 2.1** server with the **Threat Intelligence – TAXII Export** connector ([[STIX and TAXII Connectors]]).
2. Intel explorer → select STIX objects → **Export** → choose server → Export.
3. **View export history** in the Exports column.

- **Irreversible.** Make sure you own the data or are authorized to share it, and consider the destination's geographic/regulatory region.
- It's a **bulk operation**. A failed bulk export **pauses later exports** until you remove the failed operation from bulk operations history.
- Purpose: **bi-directional sharing** without custom playbooks.

---

## Common Exam Confusion

| Compare | Difference |
| --- | --- |
| **Ingestion rules vs. analytics rules** | Ingestion rules shape **TI objects before they're stored** (filter/edit). Analytics rules **match stored TI against logs** to create alerts/incidents. |
| **Ingestion rules (TI) vs. automation rules** | TI ingestion rules act on incoming intel. [[Playbooks and Automation Rules\|Automation rules]] act on incidents. |
| **Tags vs. relationships** | Tags are ad-hoc grouping (e.g. per incident). Relationships are a structured STIX link, used for a known actor or campaign. |
| **Intel explorer vs. Threat Analytics** | Intel explorer searches **both** TI objects and Threat analytics reports. Threat Analytics is Microsoft's curated campaign reporting with your exposure. See [[Threat Intelligence]]. |
| **`ThreatIntelIndicators` vs. `ThreatIntelligenceIndicator`** | New STIX-schema table vs. legacy table (ingestion stopped July 31, 2025) |
| **Created vs. ingested object edits** | Created: all fields. Ingested: tags/expiration/confidence/revoked only. |

---

## AZ-500 overlap

You know Sentinel analytics rules, workbooks and KQL hunting. New here: the TI **curation layer** (ingestion rules, STIX relationships, TLP) and **export**.

---

## What's new in SC-100

- Design TI **governance**: TLP on objects, ingestion rules as quality gates, authorization checks before export.
- Design for **cost**: ingestion rules reduce stored TI, and all TI is re-ingested every 7–10 days at standard rates.
- Design for **sharing**: import + TAXII 2.1 export = consortium-grade bi-directional intel.

---

## Exam Tips

- Manage TI → **Microsoft Sentinel Contributor**.
- Defender portal location → **Threat intelligence > Intel explorer**.
- Ingestion rules = **connectors only**, ordered low→high, **Delete** = skip, up to **15 min** to apply.
- Known actor/campaign → **relationship**, not tag.
- Indicator table → **`ThreatIntelIndicators`**.
- Export → **TAXII 2.1 only**, irreversible.

---

## Keywords

- Intel explorer, Threat intelligence page, TI object, Add and duplicate
- STIX objects: indicator, threat actor, attack pattern, identity, relationship
- Traffic Light Protocol (TLP) White/Green/Amber/Red, sensitivity
- Ingestion rules, Valid until, Extend by, Confidence, Delete action, order
- Relationship builder: uses, indicates, targets, attributed to, impersonates
- Tags, naming convention, Revoked, Expiration date
- ThreatIntelIndicators, ThreatIntelObjects, ThreatIntelligenceIndicator (legacy)
- GeoLocation, WhoIs enrichment
- Threat Intelligence workbook, TI analytics rule templates, matching analytics
- Export TI, TAXII 2.1, export history, bulk operation

---

## Related Services

- [[Threat Intelligence]] — hub: sources and TI vs. Threat Analytics
- [[STIX and TAXII Connectors]] — import feeds + TAXII Export target
- [[Threat Intelligence Platform (TIP) Connector]] — legacy push source / Upload API
- [[Microsoft Sentinel]]
- [[Security Operations]] — analytics + hunting consume this TI
- [[Playbooks and Automation Rules]]
- [[Attack Chain Models]] — attack patterns map to MITRE ATT&CK
- [[Microsoft Defender XDR]] — Intel explorer lives in the Defender portal

---

## References

- [Work with threat intelligence in Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/work-with-threat-indicators?tabs=defender-portal) — Microsoft Learn (read 2026-10-20)
- [Threat intelligence in Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/understand-threat-intelligence) — Microsoft Learn (ingestion rules, TLP, tables, lifecycle)
- [[Exam Objectives]]

---

## Verification Flag

- The Learn page's workbook example still queries the **legacy** `ThreatIntelligenceIndicator` table, but the same page says data lives in `ThreatIntelIndicators`, and the legacy table stopped ingesting July 31, 2025. Treat `ThreatIntelIndicators` as current and re-check whether the example has been updated.
- The Defender portal path was renamed to **Intel explorer** (previously "Intel management"). Confirm the name the exam uses.
