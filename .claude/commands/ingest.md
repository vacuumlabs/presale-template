---
description: Ingest a raw source into the wiki (client-inputs or team-inputs)
argument-hint: <path-to-raw-source>
---

# Ingest a raw source into the wiki

Path to ingest: **$ARGUMENTS**

Follow this workflow exactly. Do not skip steps.

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

- Add or update the source's entry in the `## Index` section of its raw-input folder's README (`client-inputs/README.md` or `team-inputs/README.md`).
- Add any new wiki pages to the categorised list in the root [`index.md`](../../index.md).

## 6. Append to log.md

Every ingest is a material wiki-changing event and is always logged. Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] ingest | <source title>

<1–2 sentence summary of what changed: pages created, pages updated, contradictions confirmed, decisions confirmed.>
```

The contradictions and decisions confirmed in step 4 are summarised inside this entry — do not add separate `contradiction` / `decision` entries for them.

## 7. Report back

Return a structured diff:

- **Created:** <list of new pages>
- **Updated:** <list of existing pages touched>
- **Contradictions:** <titles flagged into contradictions.md>
- **Index entries:** <confirm index.md + folder README updated>

Changes are committed and pushed automatically when this turn ends.
