# team-inputs/

Raw material authored by **our side of the engagement** — the Vacuumlabs team. This is primary material in the same sense as `client-inputs/`, but authored internally rather than received from the customer.

Typical contents:

- Personas the Product Owner writes
- Initial solution sketches from the Tech Lead
- Workshop outputs (whiteboard photos, journey maps, story maps)
- BD post-call debriefs (our interpretation, not verbatim client words)
- Internal estimate assumption docs
- Competitor analyses we produce ourselves

## How this differs from `client-inputs/`

| Folder | Who authored it | Edit rule |
| --- | --- | --- |
| `client-inputs/` | The customer | Immutable. Nobody edits it. New version = new dated file |
| `team-inputs/` | Someone on our team | The author owns the file. Claude may help on request (restructure, cross-link, spell-check), but never silently rewrites |

Both folders are **raw sources** in the LLM-wiki sense — Claude reads them and synthesises wiki pages from them, but neither is considered wiki output. See the [root README](../README.md) for the full pattern.

## Authoring template

Every file in this folder should carry metadata so Claude can reason about provenance and authority:

```yaml
---
type: persona | architecture-sketch | workshop-output | debrief | estimate-assumption | competitor-analysis | other
title: Human-readable title
source_type: internal-authored
author: Jane Novak
role: product-owner            # product-owner | tech-lead | delivery-lead | business-developer | pm | architect | …
status: draft | approved | superseded
version: 1
supersedes: ./older-file.md    # optional — when a new version replaces an older one
informed_by:                   # optional — client-input files that fed the author's thinking
  - ../client-inputs/2026-04-17-discovery-interview.md
last_updated: 2026-04-20
---
```

Filename convention is the same as elsewhere in the repo: date-prefix for time-bound artefacts (`2026-04-20-persona-radiologist.md`), or a slugged topic for pages that will iterate (`persona-radiologist.md` with `version:` in metadata).

## Scaffolding a new file

The fastest way is to ask Claude: *"scaffold a new PO persona for a senior radiologist in team-inputs/"* — Claude will create the file with the correct metadata pre-filled and wait for you to add the body.

## Index of team-authored inputs

<!-- Claude maintains this on every /ingest and /lint pass -->

_(empty — first file to be added)_
