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
- [Propagating template updates to live engagements](#propagating-template-updates-to-live-engagements)
- [Recipes you can add later](#recipes-you-can-add-later)

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
| **Wiki** | the five topical folders | Claude, synthesised from raw | Claude continuously; humans may edit but should prefer asking Claude so indexes stay in sync |
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

Three verbs drive everything. Only `/lint` is a slash command users type — `/ingest` is invoked implicitly by drag-and-drop (with the slash form `/ingest <path>` as a fallback for files already on disk), and asking a question runs automatically because `CLAUDE.md` routes substantive project questions through `.claude/commands/query.md`.

The operational details are in the [README's three-verbs section](README.md#the-three-verbs-ingest-ask-lint). Two mechanics worth understanding once:

### The `synthesiser` subagent

For work that would otherwise drag large volumes of source text into the main conversation — *"build the competitor wiki page from every mention across all inputs"* — Claude delegates to the `synthesiser` subagent defined in `.claude/agents/synthesiser.md`. The subagent reads the raw sources in its own context and returns only the compact diff. The main conversation stays focused on review, not retrieval. You don't invoke the subagent directly; Claude does, when the job calls for it.

### Why only `/lint` is a slash command users type

Asking a question is the high-frequency, low-friction operation — forcing users to type a `/query` slash command adds ceremony with no benefit. Dragging a file into the Claude Code panel is the same kind of natural-action trigger: it unambiguously signals *"I want this file ingested"*, so Claude internally invokes `/ingest <resolved-path>` without making the user type it. (The explicit slash form stays as a fallback when the file is already on disk.) `/lint` stays the one slash command users type because it's deliberate, infrequent (weekly), produces a dated report users may want to schedule or rerun, and isn't naturally triggered by anything else.

## Page metadata

Every wiki page and every `team-inputs/` file opens with a YAML block. The baseline fields are `type`, `title`, `tags`, `sources`, `last_updated`, `status`. Additional fields cover provenance and versioning:

- `author` + `role` — on `team-inputs/` files, records who wrote it and in what capacity.
- `version` + `supersedes` — for iterating artefacts. An old version keeps its file but flips `status: superseded`.
- `informed_by` — on `team-inputs/` files, lists the client-authored sources that fed the author. Enables contradiction detection when new client input lands.
- `canonical_source` — on a wiki page whose source of truth is a polished team-authored file; the wiki page references rather than duplicates.

Metadata is the reason Claude can filter and route without reading every body. Queries narrow by `type` and `tags`; `/lint` finds stale pages via `last_updated`; contradiction detection uses `informed_by` to trace which team artefacts depend on which client evidence. You never write it by hand — `/ingest`, the synthesiser subagent, and any *"scaffold a new …"* request fill it in for you. The complete field reference lives in [`AGENTS.md`](AGENTS.md).

## Propagating template updates to live engagements

Engagement repos are created with GitHub's **Fork** button. The fork keeps `vacuumlabs/project-template` as its upstream, but schema improvements made here (new hooks, updated commands, new merge drivers) still don't reach existing engagements until someone deliberately pulls them in — engagement repos diverge fast with client-specific content, and a blanket upstream merge would clobber that content or surface noisy conflicts.

### The `/sync-template` command

Every template-derived repo inherits `.claude/commands/sync-template.md`. In any downstream repo, a user runs:

```
/sync-template
```

Claude fetches the current `sync-manifest.json` from the template's public raw URL, compares versions, diffs every file listed in the manifest, presents a summary, and applies updates **only after explicit user confirmation**. Engagement content is never touched.

### What syncs, what stays put

The `sync-manifest.json` at the repo root is the authoritative list of schema-layer files:

- `AGENTS.md`, `CLAUDE.md`, `HOW-IT-WORKS.md`, `README.md` — routing, schema, design docs.
- `.gitattributes` — merge drivers for append-only files.
- `.gitignore` — ignore patterns for IDE metadata and per-machine Claude settings.
- `.claude/settings.json` — shared hooks (auto-sync, force-push guard, auto-commit).
- `.claude/commands/*.md` — all slash commands.
- `.claude/agents/*.md` — all subagents.
- `sync-manifest.json` itself — so the version marker advances in lockstep.

Everything else is engagement content and is never touched by sync:

- `client-inputs/`, `team-inputs/`
- Wiki folders (`contract/`, `product-management/`, `technical-architecture/`, `project-management/`, `deal-context/`)
- `index.md`, `log.md`, `decisions.md`, `contradictions.md`
- `.claude/settings.local.json` — per-machine permissions and MCP connectors

### Versioning

Semver, stored in `sync-manifest.json`, bumped **manually** when the template maintainer ships a schema change. The local repo's manifest records the version it's currently on. Comparing the two numbers tells you at a glance whether you've drifted. The manual bump is a feature, not a bug — it forces the template maintainer to pause at commit time and ask *"is this worth a sync?"*, so downstream engagements don't get nagged by noise-level changes.

### Why pull, not push

The sync is pull-based: downstream repos ask the template for updates, not the other way around. The alternative (a GitHub Action that opens PRs to every downstream repo) was considered and rejected — it requires a maintained subscriber list, a permissions setup per downstream, and a PAT with write access to every engagement repo. Pull-based keeps the template repo passive and keeps the downstream repo in control.

The trade-off is that engagements that never run `/sync-template` drift silently. Mitigate with:

- A calendar reminder on engagement kickoff ("run /sync-template monthly").
- Eventually, `/lint` can surface the version gap as part of its weekly report.

### When the downstream has customised a schema file

The sync detects the diff and surfaces it to the user with the other changes — it does not silently overwrite. The user chooses per file: keep local, take template, or cancel and reconcile manually.

If you expect to diverge from the template's schema for good reason (a client needs a bespoke hook, a command variant lives only here), keep those customisations in `.claude/settings.local.json` or in a new command file not listed in `sync-manifest.json`. Anything outside the manifest is never touched by sync.

### Bootstrapping engagements created before sync existed

Repos created from the template before `/sync-template` landed won't have the command. One-time bootstrap: in the downstream repo, ask Claude *"fetch `.claude/commands/sync-template.md` and `sync-manifest.json` from `vacuumlabs/project-template` and save them locally."* From that point `/sync-template` is available and keeps the repo in sync automatically.

## Recipes you can add later

Not configured by default. Add when you're comfortable with the basics.

### Scheduled `/lint`

*"Schedule /lint every Monday at 09:00."* Claude uses the `schedule` skill to drop a report into `project-management/lint-reports/` and (optionally) ping Slack or email.

### MCP-backed ingest

Connectors for Slack, Gmail, Jira, Confluence, Drive, and Calendar ship with Claude. Teach Claude to pull directly: *"Pull yesterday's messages from #client-x into `client-inputs/` and ingest."* Same for Confluence pages, Gmail threads, Drive files. Once you've used a variant twice, promote it to `/ingest-slack`, `/ingest-gmail`, etc.

