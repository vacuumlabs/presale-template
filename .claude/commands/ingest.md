---
description: Ingest a raw source into the wiki (client-inputs or team-inputs)
argument-hint: <path-to-raw-source>
---

# Ingest a raw source into the wiki

Path to ingest: **$ARGUMENTS**

Follow this workflow exactly. Do not skip steps.

## 0. Sync with origin

Before reading anything, run `git pull --ff-only --quiet` to pick up any pushes from teammates. If the pull fails (non-fast-forward, conflicts, network error), stop and ask the user to resolve before continuing.

## 1. Read and classify

Read the source file at `$ARGUMENTS`.

- If it is a binary (`.pptx`, `.pdf`, `.xlsx`, image), first check whether a matching `.md` summary exists next to it. If not, create one — summarising the key content in markdown — **before** continuing. The rest of the wiki will read the `.md` companion, not the binary.
- Classify provenance from the file path:
  - Under `client-inputs/` → client-authored, **immutable**. Never edit.
  - Under `team-inputs/` → team-authored. Only edit on the human author's explicit request.

## 2. Discuss before writing

Before touching any wiki pages, surface to the user:

- The key entities mentioned (people / roles, systems, features, clauses, risks, decisions).
- For each entity, whether a wiki page already exists (check `index.md` + `grep`) or whether this ingest should create one.
- Any contradictions you spotted with existing wiki pages.

Ask the user which entities to expand into wiki pages and which to leave as passing mentions. Do not assume.

For updated information that resolves open points or supersedes an older decision, ask whether to keep the old info in a note or just update the pages with the new info, replacing the old one to keep the page clean.

### 2a. Confirm unknown people

When ingesting transcripts or Slack messages, list every person mentioned or speaking in the source — full name as it appears, plus role/affiliation if given. For each name, check [`deal-context/stakeholder-map.md`](../../deal-context/stakeholder-map.md) for an existing entry, including plausible spelling variants.

- If the name matches an existing stakeholder exactly, treat it as known and proceed.
- For every name **not** found in `stakeholder-map.md`, ask the user to confirm one of:
  - **misspelling of <existing stakeholder>** — do not create a new entry; use the existing name
  - **new person** — confirm spelling and that a new entry should be created
  - **ignore** — passing mention, do not record

Do not create a new person entry or add anyone to `stakeholder-map.md` until the user has explicitly confirmed it. Do not proceed to step 3 until every unknown name has a confirmed disposition.

## 3. Update or create wiki pages

For every entity / concept the user confirms, either create a new wiki page in the correct folder (see [`AGENTS.md`](../../AGENTS.md) routing table) or update the existing one.

Every wiki page carries the metadata schema from `AGENTS.md`:

```yaml
---
type: ...
title: ...
tags: [...]
sources:
  - <relative path back to the raw source you just ingested>
  - <other raw sources that previously contributed>
last_updated: <today, YYYY-MM-DD>
status: draft | active | approved | superseded
---
```

Typical ingest touches 5–15 pages. Every factual claim on a wiki page links back to at least one raw source.

## 4. Flag contradictions (gated on user confirmation)

If the source conflicts with claims in existing wiki pages, present each conflict to the user and ask whether to record it. On confirmation, append a block to [`contradictions.md`](../../contradictions.md) with:

- **Claim A** (existing wiki page + link)
- **Claim B** (new source + link)
- **Impact** (commercial / technical / scope / legal)
- **Owner** (who should resolve)
- **Resolution:** `?` (stub)
- **First flagged:** today

A contradiction between a `team-inputs/` file and a `client-inputs/` file is almost always a discovery finding worth raising explicitly — but still ask before persisting.

If the ingest also produces a **decision** worth recording, apply the same pattern: present it, ask, and on confirmation append to [`decisions.md`](../../decisions.md).

## 5. Update indexes

For wiki pages created, renamed, or removed, follow the two-level index rule in [`AGENTS.md`](../../AGENTS.md) — update both the root `index.md` and the parent folder's `README.md`.

Ingest-specific addition: also add or update the source's entry in the `## Index` section of its raw-input folder's README (`client-inputs/README.md` or `team-inputs/README.md`).

## 6. Append to log.md

Every ingest is a material wiki-changing event and is always logged. Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] ingest | <source title>

<one sentence describing the source and the overall scope of changes>
- Created: <page> — <one phrase on what it covers>
- Updated: <page> — <what changed>
- Contradictions: <title> (or "none")
```

Use bullets when 3 or more pages were touched; collapse to prose for smaller ingests (1–2 pages).

**Important — append only at the true end of the file.** When using an edit tool that works by find-and-replace, anchor on the **last line of existing content** (the final sentence of the previous log entry), not on the previous entry's `##` heading. Anchoring on a heading and prepending the new block before it inserts the entry in the wrong position.

The contradictions and decisions confirmed in step 4 are summarised inside this entry — do not add separate `contradiction` / `decision` entries for them.

## 7. Report back

Return a structured diff:

- **Created:** <list of new pages>
- **Updated:** <list of existing pages touched>
- **Contradictions:** <titles flagged into contradictions.md>
- **Index entries:** <confirm index.md + folder README updated>
- **Missing attachments:** <any files referenced in the source that could not be retrieved — name each one, explain why it's missing, and tell the user what to do, e.g. "please share the file directly">

Changes are committed and pushed automatically when this turn ends.
