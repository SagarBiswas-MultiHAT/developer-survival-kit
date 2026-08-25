# Contributing to `research`

> *"Curiosity is my payload - delivered over POST."*

Thank you for your interest in contributing. This repository is a living collection of technical research notes. Contributions that improve accuracy, add new data sources, or extend coverage to new topics are genuinely welcome.

---

## What You Can Contribute

### 🔍 Data Corrections
If a statistic, citation, or claim in any research note is outdated or incorrect, please open an issue or PR with:
- The specific claim/line number
- Your correction
- A link to the primary source

### 📊 New Data Sources
If you have access to a study, dataset, or platform analytics report that strengthens or contradicts existing notes, share it. Preferred sources:
- Official platform analytics (Buffer, Sprout Social, HubSpot, native platform Insights)
- Peer-reviewed papers
- Reproducible datasets with methodology disclosure

### 🌍 Regional Audience Data
Most research here is calibrated for **BST (UTC+6, Bangladesh Standard Time)** audiences. If you have data for other time zones or regional communities (e.g., IST, SGT, PST), open an issue - region-specific addendums are welcome.

### 🗂️ New Research Notes
If you want to contribute an entirely new research note, open an issue first with the proposed topic and key sources. Notes must:
- Be placed in the `notes/` directory (e.g. `notes/XSS-CHEATSHEET.md`)
- Cite at least one verifiable primary source
- Be written in Markdown
- Follow the metadata header format (see existing notes)

---

## How to Contribute

### Option A: Open an Issue
For corrections, suggestions, additional data, or discussion questions - [open a GitHub Issue](../../issues/new).

Use one of these labels (apply manually or request in the issue body):
- `correction` - factual error or outdated data
- `enhancement` - improvement to structure, clarity, or depth
- `new-note` - proposal for a new research document
- `data-source` - new supporting data or study

### Option B: Submit a Pull Request

1. Fork the repository
2. Create a branch: `git checkout -b fix/posting-time-correction` (or `feat/new-note-topic`)
3. Make your changes
4. Commit with a clear message: `fix: correct LinkedIn peak hour citation (Buffer 2026)`
5. Open a PR against `main` - describe what changed and why

---

## Research Note Format

New notes should follow this header format:

```markdown
# [TITLE]

**Author:** [Your name or @handle]
**Version:** 1.0
**Last Updated:** YYYY-MM-DD
**Status:** Draft | Final
**Audience:** [e.g., CSE students, security practitioners, BST developers]

---

## TL;DR

- Bullet 1
- Bullet 2
- Bullet 3

---

## [Content sections...]

---

## Sources

[1]: URL "Description"
[2]: URL "Description"
```

---

## Code of Conduct

- Be direct and factual - cite sources, not opinions
- Critique the data, not the person who collected it
- Respect that research notes may be intentionally narrow in scope (BST-specific, community-specific, etc.)

---

## Questions?

Open an issue or start a [Discussion](../../discussions) - feedback threads, regional data shares, and methodology debates are all fair game.
