# Project Purpose

This repository contains concise study notes for Microsoft's SC-100 certification.

The goal is to pass the exam while building a public GitHub knowledge base.

Do not write tutorials.

Do not write textbook explanations.

Write concise GitHub Markdown suitable for quick revision.

---

# Audience

The reader has already studied AZ-500.

Avoid explaining basic Azure security concepts unless they are required for SC-100.

Focus on architectural decisions.

---

# Writing Style

Use:

- Markdown
- Bullet points
- Tables where useful
- One topic per file

Avoid:

- Long paragraphs
- Marketing language
- Repeating Microsoft Learn wording

---

# Every note should contain

# Title

## Purpose

One sentence.

## Why architects choose it

Architecture decisions.

## When to use

Bullet list.

## When NOT to use

Bullet list.

## Exam Tips

Only high-value facts.

## AZ-500 overlap

Explain what I already know.

## What's new in SC-100

Only new architectural knowledge.

## Related Services

Use Obsidian wiki links.

Example

[[Conditional Access]]

[[PIM]]

[[Microsoft Defender for Cloud]]

## Common Exam Confusion

Explain confusing comparisons.

Whenever two Microsoft services solve similar problems, always include a comparison table.

Example

Azure Firewall vs NSG

Defender for Cloud vs Azure Policy

Private Link vs Service Endpoint

Managed Identity vs Service Principal

Sentinel vs Defender XDR

Purview vs Information Protection

## Keywords

Bullet list of common keywords and phrases that should trigger recall of this concept in the exam.

---

# Obsidian

Always create wiki links.

Example

[[Conditional Access]]

Never use normal hyperlinks for internal notes.

Every important Azure service must have its own page.

Whenever a service is mentioned, create an Obsidian wiki link.

Example

[[Microsoft Sentinel]]

[[Conditional Access]]

[[PIM]]

[[Azure Firewall]]

If the linked page doesn't exist yet, leave the wiki link anyway.

Whenever a note is modified, look for opportunities to create new wiki links. The graph should become denser over time.

---

# Maintenance

Whenever a new service note is created, also update [Cheat Sheets/services.md](Services.md) with one line describing it.

Keep [Cheat Sheets/Architecture Decisions.md](Cheat%20Sheets/Architecture%20Decisions.md) up to date as a decision-tree style cheat sheet, e.g.:

```
Need SIEM?
  ↓
Microsoft Sentinel

Need endpoint protection?
  ↓
Defender XDR

Need cloud posture?
  ↓
Defender for Cloud
```

---

# Knowledge Graph Rules

This repository should behave like a graph rather than a collection of isolated documents.

Every note should contain at least:

- 3 incoming conceptual links
- 3 outgoing conceptual links

Whenever introducing a new concept:

1. Link to existing notes.
2. Update existing notes to link back where appropriate.
3. Strengthen the Obsidian knowledge graph over time.

Do not leave orphan notes unless the concept is completely standalone.

---

# GitHub

Markdown must render correctly on GitHub.

---

# Existing Notes

Before creating a new note:

Search the repository.

If a note already exists, update it instead of creating duplicates.

---

# Accuracy

Prefer Microsoft Learn terminology.

Do not invent architecture guidance.

Flag anything that requires verification.
