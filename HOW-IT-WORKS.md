# How this works

A companion to [`README.md`](README.md). The README tells you *what to do*; this doc explains *why the repo is shaped the way it is* and covers the mechanics a user doesn't need on day one but will want the first time something surprising happens.

## Contents

- [The problem it solves](#the-problem-it-solves)
- [The pattern: an LLM wiki](#the-pattern-an-llm-wiki)
- [Why a Git repo, not a Claude project](#why-a-git-repo-not-a-claude-project)
- [Three layers, three edit rules](#three-layers-three-edit-rules)
- [How the raw layer really works](#how-the-raw-layer-really-works)
- [How Claude keeps the wiki synced](#how-claude-keeps-the-wiki-synced)
- [Page metadata](#page-metadata)
- [Scaffolding wiki pages at kickoff](#scaffolding-wiki-pages-at-kickoff)
- [The revert principle](#the-revert-principle)
- [On the roadmap](#on-the-roadmap)

## The problem it solves

Project context today lives scattered across Slack, Drive, email, decks, Jira, Confluence, and individual heads. When context isn't in one place:

- onboarding a new person (or an AI agent) is slow and lossy,
- decisions get re-litigated because nobody can find the previous answer,
- client questions take hours of archaeology,
- critical details — a contract clause, an architectural assumption — silently go missing.

Consolidating into a single, version-controlled place fixes the scatter. But a folder of documents isn't enough on its own: it still has to be readable, cross-linked, and up to date. That's what the LLM-wiki layer does.

## The pattern: an LLM wiki

Structured after [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): **raw sources stay immutable, an LLM synthesises them into cross-linked pages, and every answer is grounded in those pages with citations.** The structure gives two properties a pile of documents doesn't have:

- **Grounding.** Every factual claim on a wiki page links back to at least one raw source. No claim is free-floating.
- **Compounding.** Each ingested source strengthens every existing page. Each answered question can be refiled as a new page. The longer the engagement runs, the better the wiki answers the next question — the opposite of starting from zero each time.

## Why a Git repo, not a Claude project

We previously kept this material inside a Claude project. Moving it into a Git repository gives properties a Claude project can't:

- **Resilience.** Files live outside Claude. If Claude is down or a workspace breaks, the knowledge is still there, still readable, still editable with any tool.
- **Single folder on disk.** Everything in one clonable, backupable, greppable place instead of split between a Claude workspace and wherever the "real" files live.
- **Full history.** Git records who changed what and when — audit trail, blame, time-travel to previous state.

## Three layers, three edit rules

Every file in this repo belongs to exactly one of three layers, and the layer dictates who may edit it.

| Layer | Where | Who writes | Who edits |
| --- | --- | --- | --- |
| **Raw** | `client-inputs/`, `team-inputs/` | Customer or team member | See next section |
| **Wiki** | the six topical folders | Claude, synthesised from raw | Claude continuously; humans may edit but should prefer asking Claude so indexes stay in sync |
| **Schema** | `AGENTS.md`, `CLAUDE.md` | Repo maintainer | Rarely — changes the contract |

The **schema** layer is worth isolating: `AGENTS.md` defines routing, operations, metadata, and the edit contract Claude follows every session. `CLAUDE.md` routes substantive project questions through the wiki workflow. Editing these changes how every future ingest and answer behaves, so they are explicitly maintainer-scoped rather than day-to-day edits.

## How the raw layer really works

### `client-inputs/` — from the customer

Anything the client sends: transcripts, RFPs, emails, annexes, decks, screenshots, filled-in forms.

- **Immutable.** Not humans, not Claude. An updated version lands as a *new* date-prefixed file — the old one stays as the historical record.
- **Binary companion rule.** Every non-markdown file (PDF, deck, spreadsheet, image) gets a matching `.md` summary next to it. Without it, Claude can't answer questions about the binary.

### `team-inputs/` — authored by our side

Primary material we write ourselves: PO personas, Tech Lead sketches, workshop outputs, BD debriefs, estimate assumptions, internal competitor analyses.

- **The author owns the file.** The PO iterates on their persona, the architect on their sketch. Claude may help on request (restructure, spell-check, add cross-links, scaffold a new version) but never silently rewrites.
- **Contradictions with `client-inputs/` are findings, not bugs.** When a PO persona says X and a discovery interview says Y, that's real signal — Claude flags it into `contradictions.md` at ingest time rather than reconciling it away.

### Polished team-authored pages can *be* the canonical page

Sometimes a PO writes a persona that's already wiki-shaped — well-structured, polished, ready to read. In that case don't duplicate it under `product-management/personas/`. The wiki page uses `canonical_source:` in its metadata to point at the `team-inputs/` file and adds synthesis around it (other mentions, contradictions, cross-links). The author keeps ownership; the wiki adds navigation.

## How Claude keeps the wiki synced

Three verbs drive everything. `/ingest` and `/lint` are explicit slash commands; asking a question runs automatically because `CLAUDE.md` routes substantive project questions through `.claude/commands/query.md`.

The operational details are in the [README's Operations section](README.md#operations-ingest-ask-lint). Two mechanics worth understanding once:

### The `synthesiser` subagent

For work that would otherwise drag large volumes of source text into the main conversation — *"build the competitor wiki page from every mention across all inputs"* — Claude delegates to the `synthesiser` subagent defined in `.claude/agents/synthesiser.md`. The subagent reads the raw sources in its own context and returns only the compact diff. The main conversation stays focused on review, not retrieval. You don't invoke the subagent directly; Claude does, when the job calls for it.

### Why `/query` is implicit but `/ingest` and `/lint` are explicit

Asking questions is the high-frequency, low-friction operation — forcing users to type `/query` adds ceremony. The routing in `CLAUDE.md` gives substantive project questions the full cited-answer workflow without asking the user to opt in. `/ingest` and `/lint` stay explicit because they change the wiki state, should be deliberate, and log actions that need to be reviewable.

## Page metadata

Every wiki page and every `team-inputs/` file opens with a YAML block. The baseline fields are `type`, `title`, `tags`, `sources`, `last_updated`, `status`. Additional fields cover provenance and versioning:

- `author` + `role` — on `team-inputs/` files, records who wrote it and in what capacity.
- `version` + `supersedes` — for iterating artefacts. An old version keeps its file but flips `status: superseded`.
- `informed_by` — on `team-inputs/` files, lists the client-authored sources that fed the author. Enables contradiction detection when new client input lands.
- `canonical_source` — on a wiki page whose source of truth is a polished team-authored file; the wiki page references rather than duplicates.

Metadata is the reason Claude can filter and route without reading every body. Queries narrow by `type` and `tags`; `/lint` finds stale pages via `last_updated`; contradiction detection uses `informed_by` to trace which team artefacts depend on which client evidence. You never write it by hand — `/ingest`, the synthesiser subagent, and any *"scaffold a new …"* request fill it in for you. The complete field reference lives in [`AGENTS.md`](AGENTS.md).

## Scaffolding wiki pages at kickoff

The README's kickoff checklist lists the wiki pages each role should create in week one. You don't have to write them one by one. Any role can ask Claude to scaffold their whole set in one shot:

> *"I'm the PO — scaffold the product-management wiki pages for kickoff against the current `engagement.md`."*

Claude creates empty pages with the right metadata and leaves `TODO` bodies for the author to fill. Missing wiki pages are worse than 3-sentence stubs, because `/ingest` needs something to hang cross-links on — scaffolding gets you to "at least stubs" in a single prompt.

## The revert principle

The README's revert table covers the common situations. The underlying rule: **the further a change has travelled (working copy → commit → pushed), the more work it takes to undo.** Catching mistakes before `push` is always cheaper. For secrets this is not just a convenience rule — it's the difference between "rotate later" and "rotate now, assume it's been seen."

## On the roadmap

Not configured yet; worth adding when someone has the time.

### Git pre-commit metadata validation

A hook that blocks commits adding wiki pages with invalid metadata. Catches drift mechanically at commit time rather than waiting for weekly `/lint` to surface it. Particularly valuable once the wiki has 50+ wiki pages and manual edits start sneaking in.
