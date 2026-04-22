# Project Knowledge Base — LLM Wiki schema

This repository is the single source of truth for project knowledge. It is structured as an **LLM wiki** (after Karpathy's pattern): raw sources are immutable, synthesised pages are LLM-owned and continuously maintained, and everything is cross-linked so questions get grounded answers instead of guesses.

Start every session by reading [`index.md`](./index.md). It is short and it is the map.

## Layers

This repo has three layers. Every file belongs to exactly one of them.

1. **Raw sources (immutable by default).**
   - [`client-inputs/`](./client-inputs/) — material from the customer (transcripts, RFPs, emails, annexes, decks they sent).
   - [`team-inputs/`](./team-inputs/) — primary material we authored ourselves (PO personas, architect sketches, workshop outputs, BD debriefs, estimate assumptions).
2. **Wiki (LLM-owned).** The six topical folders below. Claude creates and maintains these pages by synthesising across the raw layer. Humans may edit but should prefer asking Claude, so `index.md` and `log.md` stay in sync.
3. **Schema.** This file (`AGENTS.md`) defines routing, operations, metadata, and the edit contract. Claude reads it every session.

## Routing

| Topic / question type | Directory |
| --- | --- |
| Contract terms, MD budget, commercials, SOW, billing, legal, engagement metadata | [`contract/`](contract/) |
| Sales context, account management, client sentiment, client motivation, stakeholder map, target audience | [`deal-context/`](deal-context/) |
| System design, architecture diagrams, tech stack decisions, ADRs, ARB records, integrations | [`technical-architecture/`](technical-architecture/) |
| Personas, PRDs, user journeys, feature maps, product strategy, domain & competitive briefs, problem framing, competitor analysis | [`product-management/`](product-management/) |
| Roadmaps, timelines, milestones, capacity planning, status reports, project assembly, debriefs, post-mortems, scope definition, work breakdown, estimates, assumptions, change requests | [`project-management/`](project-management/) |
| Meeting notes, user feedback, raw client communications, transcripts | [`client-inputs/`](client-inputs/) |
| Team-authored raw material (PO personas, architect sketches, workshop outputs, BD debriefs, estimate assumptions) | [`team-inputs/`](team-inputs/) |

Each folder has its own `README.md` with the typical wiki pages and its local index.

## Operations

Three verbs drive the wiki. Two are explicit slash commands; the third — answering questions — runs automatically whenever the user asks a substantive project question (see [`CLAUDE.md`](./CLAUDE.md)).

- **[`/ingest <path>`](./.claude/commands/ingest.md)** — integrate a new raw source. Creates or updates wiki pages (typically 5–15 per source), flags contradictions, updates indexes, appends to `log.md`.
- **Answering a question** — no slash command needed. Claude follows the workflow in [`.claude/commands/query.md`](./.claude/commands/query.md): reads `index.md` first, prefers wiki pages over raw sources, cites every factual claim, names gaps honestly, flags contradictions, and offers to refile reusable answers as new wiki pages. This is how the wiki compounds.
- **[`/lint`](./.claude/commands/lint.md)** — periodic health check. Surfaces unresolved contradictions, stale pages, orphans, index drift, missing wiki pages, provenance holes, binary-companion gaps, and overdue TODOs.

A [`synthesiser` subagent](./.claude/agents/synthesiser.md) handles heavy multi-source synthesis so the main thread doesn't balloon with raw source text.

## Edit contract

| Origin | Who edits | Claude's role |
| --- | --- | --- |
| `client-inputs/` (client-authored) | Never edited — immutable. New versions are committed as new dated files | Read-only. Never writes |
| `team-inputs/` (team-authored) | The human author | May propose edits, restructure, spell-check, cross-link **only on the author's request**. Never silently rewrites |
| Wiki folders (LLM-owned) | Claude, continuously | Writes and maintains. Humans may edit but should prefer asking Claude |

A contradiction between `team-inputs/` and `client-inputs/` is usually a discovery finding — flag it, don't silently reconcile.

## Metadata schema

Every wiki page and every file under `team-inputs/` carries YAML metadata. Minimum fields:

```yaml
---
type: stakeholder | persona | competitor | system | integration | adr | epic | decision | status | risk | clause | concept | ...
title: Human-readable title
tags: [tag1, tag2]
sources:
  - ../client-inputs/2026-04-17-kickoff-transcript.md
  - ../team-inputs/2026-04-20-persona-radiologist.md
last_updated: YYYY-MM-DD
status: draft | active | approved | superseded
---
```

Additional fields, used where relevant:

- `source_type: client | internal-authored` — on raw files in `team-inputs/` only, distinguishes team-authored from client-authored.
- `author` + `role` — on `team-inputs/` files, records who wrote it and in what capacity.
- `version` + `supersedes: ./old.md` — for iterating artefacts. Old version keeps its file but flips `status: superseded`.
- `canonical_source: ../team-inputs/...` — on a wiki page whose source of truth is a polished team-authored file (e.g. a PO persona). The wiki page references rather than duplicates.
- `informed_by: [...]` — on `team-inputs/` files, lists the client-authored sources that fed the author. Enables contradiction detection when new client input lands.

## Navigation files

Two files drive discovery. Both live at the repo root.

- [`index.md`](./index.md) — categorised catalogue of every wiki page. Claude reads this first when answering any question.
- [`log.md`](./log.md) — append-only audit trail. Format: `## [YYYY-MM-DD HH:MM] <verb> | <title>` followed by one or two sentences. Verbs: `ingest`, `query`, `lint`, `decision`, `author`.

Two cross-cutting pages also live at the root.

- [`decisions.md`](./decisions.md) — every meaningful call made across the engagement, with links back to the relevant ADR, clause, or change request.
- [`contradictions.md`](./contradictions.md) — conflicting claims across sources until someone resolves them. Every entry carries both sides and a `Resolution: ?` stub.

## Automations configured in this repo

Two `PreToolUse` hooks live in [`.claude/settings.json`](.claude/settings.json):

- **Auto-sync before state-changing actions.** Before every `Bash`, `Edit`, `Write`, or `NotebookEdit` call, the harness runs `git fetch` + `git pull --ff-only` (silent, non-blocking). Assume the working copy is current; don't add your own pulls.
- **Force-push to `master` is blocked.** Any `Bash` command containing `git push` together with `--force`, `-f`, or `--force-with-lease` is denied while `master` is checked out. Don't try to work around it — if the user genuinely needs one, ask them to remove the hook temporarily.

Optional automations users may add (see the root README):

- A weekly scheduled `/lint` job.
- MCP-backed ingest variants that pull from Slack / Gmail / Jira / Confluence / GDrive into `client-inputs/` or `team-inputs/`.

## Conventions

- **Markdown for narrative.** Prefer Markdown for anything authored inside the repo.
- **Date-prefix time-bound files (required).** Use `YYYY-MM-DD-*.md` (e.g. `2026-04-13-kickoff-notes.md`) for any meeting note, status update, or other time-bound artefact.
- **Every binary needs a matching `.md` summary (required).** For any non-Markdown file (PDF, deck, spreadsheet, image), commit a matching `.md` next to it with a short summary. Without this, Claude can't answer questions about the binary.
- **External systems: snapshot + link.** Decision-grade artefacts that live in Jira, Confluence, Drive or similar (signed SOW, approved PRD, architecture diagrams) should be snapshotted into the repo *and* linked back to the source. Non-decision-grade material can be pointer-only.
- **Engagement metadata.** Canonical engagement metadata lives in [`contract/engagement.md`](contract/engagement.md).
- **Place by primary question.** When adding a new artefact, put it in the directory that matches the *primary* question it answers, even if it touches several.
- **Metadata on every wiki page and every `team-inputs/` file.** See the schema above.
- **Prose over bullets on wiki pages.** Use prose paragraphs. Tables only when the content is inherently tabular. Follows the project's house style.
- **Cite or say the gap.** Every factual claim on a wiki page should link back to at least one raw source. If a claim isn't sourced, flag it — don't smooth it over.
