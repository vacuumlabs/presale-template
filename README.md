# Project Template

A template repo for running an AI-assisted SDLC against a client engagement — **across the entire lifecycle**: presales, discovery, scope, architecture, delivery, and close-out. It is the project's **LLM wiki** — raw sources stay immutable, Claude synthesises them into cross-linked pages, and questions read the wiki instead of re-discovering knowledge each time.

**Not a code repo.** Code itself lives in a separate repository created at delivery kickoff (in the customer's GitHub organisation if they prefer, or in Vacuumlabs's otherwise). From that point the two repos run in parallel for the rest of the engagement — see [When coding starts](#when-coding-starts).

**UI: Claude Code inside the Claude app, for everyone.** All roles drive this repo through [Claude Code](https://claude.com/claude-code) **running inside the Claude desktop app** — not the terminal CLI, not the Claude web app. Install + SSH setup at the bottom in [Setup](#setup).

See [`AGENTS.md`](AGENTS.md) for routing, metadata schema, and edit contract. See [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md) for the design rationale and deeper mechanics.

## Who uses this repo

Three roles actively drive the wiki on a typical engagement. Each has a dedicated section below — scroll to yours.

- [**Project Manager (PM)**](#if-youre-the-project-manager) — contract, commercials, stakeholders, roadmap, risks, weekly status
- [**Product Owner (PO)**](#if-youre-the-product-owner) — problem framing, personas, features, scope, estimates
- [**Tech Lead**](#if-youre-the-tech-lead) — architecture, systems, integrations, ADRs, technical scope

Shared essentials — [Operations](#shared-operations-ingest-ask-lint), [When coding starts](#when-coding-starts), [file layout](#file-layout), [metadata](#metadata-in-one-minute), [Setup](#setup) — are at the bottom.

## If you're the Project Manager

**Your folders:** [`project-management/`](project-management/), [`contract/`](contract/), [`deal-context/`](deal-context/)

**Raw sources you bring in**

- → `client-inputs/`: signed SOW, client emails, meeting transcripts, steering-committee decks, client-sent annexes
- → `team-inputs/`: sales-call debriefs, pricing assumptions, your estimate-assumption doc

**Wiki pages you maintain**

- Engagement: [`contract/engagement.md`](contract/engagement.md), `contract/commercials.md`, `contract/clauses/<name>.md` (audit, exit, liability, data-residency, IP, SLA)
- Project: `project-management/roadmap.md`, `milestones.md`, `risks.md`, `capacity.md`, weekly `status/YYYY-MM-DD-status.md`
- Scope & estimates (co-owned with PO + Tech Lead, under `project-management/`): `work-breakdown.md`, `epics/<name>.md`, `assumptions.md`, `change-requests.md`, `out-of-scope.md`, `estimate-methodology.md`
- Deal context: `deal-context/stakeholder-map.md`, `deal-context/stakeholders/<name>.md`, `client-motivation.md`, `client-sentiment.md`

**Scaffold your pages in one prompt (week 1)**

> *"I'm the PM — scaffold the PM wiki pages against the current `contract/engagement.md`."*

Stubs are fine; `/ingest` needs something to hang cross-links on.

**Daily / weekly rhythm**

- **After each client meeting** — drag transcript/notes straight into Claude Code and ask to ingest into `client-inputs/`. Slack/Gmail/Jira can feed `client-inputs/` directly via the MCP connectors (see [Optional next steps](#optional-next-steps)).
- **Decisions surface** — ask Claude to append to [`decisions.md`](decisions.md) so the *why* is recoverable.
- **Risks surface** — append a line to `risks.md` (owner, likelihood, impact, mitigation).
- **Weekly** — draft a new `status/YYYY-MM-DD-status.md` (ask Claude to pull the last 7 days from `log.md`). Monday: run `/lint`.
- **Before steering committee** — ask Claude for a one-paragraph commercials + sentiment summary.

**Prompts you'll use often**

- *"Ingest `client-inputs/2026-05-03-steering.md`."*
- *"Summarise the last 4 weeks — commercials, progress, risks — for a steering slide."*
- *"Which risks have no owner?"*
- *"Record a decision: approved change request CR-012 on 2026-05-01, cost 20 MDs."*
- *"Draft this week's status from the log."*

## If you're the Product Owner

**Your folders:** [`product-management/`](product-management/); co-owns scope & estimates pages under [`project-management/`](project-management/)

**Raw sources you bring in**

- → `client-inputs/`: discovery-interview notes, client-sent PRDs, user-testing recordings, feedback sessions
- → `team-inputs/`: polished personas (often the canonical page — see [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md#how-the-raw-layer-really-works)), problem framing, competitor briefs, workshop outputs

**Wiki pages you maintain**

- Product: `product-management/problem-statement.md`, `personas/<name>.md`, `pain-points.md`, `journeys/<name>.md`, `features/<name>.md`, `competitors/<name>.md`, `domain-brief.md`
- Scope (under `project-management/`, co-owned with Tech Lead): `work-breakdown.md`, `epics/<name>.md`, `assumptions.md`, `out-of-scope.md`, `estimate-methodology.md`, `open-questions.md`

**Scaffold your pages in one prompt (week 1)**

> *"I'm the PO — scaffold the product-management wiki pages and the scope & estimates pages under project-management against the current `contract/engagement.md`."*

**Daily / weekly rhythm**

- **Discovery interview ends** — drag notes into Claude Code and ask to ingest into `client-inputs/`.
- **You write a persona or domain brief** — place in `team-inputs/` (stays yours to iterate). Claude creates a `product-management/personas/<name>.md` with `canonical_source:` pointing at your file.
- **Workshop outputs land** — drag photos + summary into Claude Code and ask to ingest into `team-inputs/`.
- **Open questions pile up** — track in `project-management/open-questions.md`; promote unresolved ones to `contradictions.md` with Claude's help.
- **Before backlog grooming** — ask which features still need acceptance criteria.

**Prompts you'll use often**

- *"Scaffold a new persona in `team-inputs/` for a senior end-user."*
- *"Which pain points appear in more than one persona?"*
- *"List every feature in scope for the current release, with its owner and acceptance-criteria status."*
- *"Which open questions are blocking the current estimate?"*
- *"Walk a new joiner through the product on one page."*

## If you're the Tech Lead

**Your folders:** [`technical-architecture/`](technical-architecture/); co-owns scope & estimates pages under [`project-management/`](project-management/)

**Raw sources you bring in**

- → `client-inputs/`: architecture diagrams, integration specs, security questionnaires the client filled in, SaaS vendor docs they share
- → `team-inputs/`: your solution sketch, whiteboard photos from architecture workshops, reference-architecture notes, proof-of-concept writeups

**Wiki pages you maintain**

- Architecture: `technical-architecture/tech-stack.md`, `systems/<name>.md` per subsystem, `integrations/<name>.md` per external integration, `adrs/ADR-NNNN-<slug>.md` per material decision, `arb-records/YYYY-MM-DD-<topic>.md` per ARB, `diagrams/<name>.md`
- Scope (with PO): sizing, technical assumptions, technical out-of-scope

**Scaffold your pages in one prompt (week 1)**

> *"I'm the Tech Lead — scaffold the technical-architecture wiki pages against the current `contract/engagement.md`."*

**Daily / weekly rhythm**

- **Architecture workshop ends** — drag whiteboard photos / diagram exports into Claude Code and ask to ingest into `team-inputs/` (Claude creates the matching `.md` summary).
- **A decision is imminent** — scaffold an ADR (*context, options, decision, consequences*); ask Claude to cross-link related systems and integrations.
- **Client sends an architecture doc** — drag it into Claude Code and ask to ingest into `client-inputs/` (matching `.md` summary handled for binaries).
- **New integration proposed** — `integrations/<name>.md` with owner, contract, SLA, failure modes.
- **Before ARB** — ask Claude for the integration map and the ADRs you need to defend.

**Prompts you'll use often**

- *"Scaffold ADR-0005 on the integration-layer business-logic boundary."*
- *"Which integrations have no clear owner?"*
- *"Walk me through the MVP happy path end-to-end across all systems."*
- *"What technical assumptions would break if we swapped the payments provider?"*
- *"List every ADR, with its status and the systems it touches."*

## Shared operations: ingest, ask, lint

Three verbs drive the wiki regardless of role. Two are explicit slash commands; the third — asking — runs automatically when you pose a substantive project question.

**Ingesting a raw source** — **the default flow is drag-and-drop**: drag the file (transcript, PDF, spreadsheet, screenshot, deck, image) straight into the Claude Code panel in the Claude app and ask *"ingest this into `client-inputs/`"* or *"… into `team-inputs/`"*. Claude places the file in the right folder, creates a matching `.md` summary if it's a binary, then runs the full ingest workflow: reads the source, discusses takeaways with you, creates or updates 5–15 wiki pages, flags contradictions into `contradictions.md`, updates `index.md` and folder READMEs, appends to `log.md`, and reports a structured diff for your review. Run one source at a time.

If the file is already on disk, you can also call `/ingest <path>` explicitly — same workflow.

**Ask the wiki** — just ask in plain English. Substantive project questions auto-follow a fixed workflow: read `index.md` first, drill into the relevant wiki pages, answer **with citations**, and — if the answer is likely to be re-asked — offer to refile it as a new page. If the wiki lacks the information, Claude says so. Meta-questions about the tooling are answered directly.

**`/lint`** — weekly health check. Scans for unresolved contradictions, stale pages (`last_updated` >14 days on active engagements), orphan pages, missing wiki pages (entities mentioned 3+ times with no page), index drift, provenance holes, binary-companion gaps, overdue TODOs, invalid metadata. Output lands at `project-management/YYYY-MM-DD-lint-report.md`.

Routing lives in [`CLAUDE.md`](CLAUDE.md); full operation details in [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md#how-claude-keeps-the-wiki-synced).

## When coding starts

This wiki covers the **whole engagement** — presales, discovery, scope, architecture, delivery, close-out. **Code itself belongs in a separate repository**, created at delivery kickoff in the customer's GitHub organisation if they prefer, or in Vacuumlabs's otherwise. From that point the two repos run in parallel: the code repo accumulates implementation and tests; this wiki continues to absorb client communications, status updates, risks, change requests, new ADRs, and decisions through to close-out.

**What typically travels to the code repo at delivery kickoff**

- `technical-architecture/adrs/` — copied or referenced into the code repo's `docs/adrs/`
- `technical-architecture/integrations/` — interface specs the developers need on day one
- `technical-architecture/systems/` — subsystem pages that constrain implementation
- Key entries from `decisions.md` — especially decisions with implementation impact
- A one-page engagement summary — so the code repo carries the context of *why*

**What stays in this wiki, for the life of the engagement**

- All raw inputs (`client-inputs/`, `team-inputs/`) — the audit trail
- Commercial context, contract, stakeholders, client sentiment
- Scope, estimates, change requests, risks, weekly status
- Ongoing decisions, contradictions, retrospective notes

**Generate the handoff pack in one prompt**

> *"Generate a code-repo handoff pack: list the ADRs, integration specs, systems pages, and decisions that should travel to the new code repo, with the paths and a one-line summary of each."*

## File layout

```
.
├── AGENTS.md              ← schema for Claude: routing, operations, metadata, edit contract
├── CLAUDE.md              ← routes substantive project questions through the wiki workflow
├── HOW-IT-WORKS.md        ← design rationale and deeper mechanics
├── README.md              ← this file
├── index.md               ← catalogue of every wiki page (Claude reads first)
├── log.md                 ← audit trail: ingest / ask / lint
├── decisions.md           ← decisions with links back to the artefacts that made them
├── contradictions.md      ← conflicting claims pending resolution
│
├── client-inputs/         ← raw — from the customer, immutable
├── team-inputs/           ← raw — authored by our team (owner-iterated)
│
├── contract/              ← PM: contract, MD budget, commercials, SOW, legal
├── deal-context/          ← PM: sales context, stakeholders, sentiment
├── product-management/    ← PO: personas, PRDs, features, competitors
├── technical-architecture/← Tech Lead: systems, ADRs, integrations
├── project-management/    ← PM (+ PO & Tech Lead for scope): roadmap, risks, status, debriefs, scope, estimates, assumptions, CRs
│
└── .claude/
    ├── commands/ingest.md ← /ingest slash command
    ├── commands/query.md  ← workflow for answering questions (auto-invoked via CLAUDE.md)
    ├── commands/lint.md   ← /lint slash command
    ├── agents/synthesiser.md ← subagent for heavy multi-source synthesis
    └── settings.json      ← git auto-sync + force-push guard hooks
```

Each topical folder carries its own `README.md` with typical wiki pages and a local index.

### Raw inputs — two folders, two rules

The separation reflects different **provenance** and different **authority**: what the customer said (evidence we received) is not the same as what we wrote (interpretation we produced). Keeping them in different folders lets Claude reason about whose claim is whose, preserves an audit trail that will stand up years later, and — crucially — lets disagreements between the two surface as **findings** rather than being silently reconciled. A PO persona that diverges from a client interview is signal, not noise. Confusing the two breaks that contract.

- **`client-inputs/` — from the customer.** Transcripts, RFPs, emails, annexes, decks, filled-in forms. **Immutable** — nobody edits, not humans, not Claude. Updates land as *new* date-prefixed files. Every non-markdown file gets a matching `.md` next to it (e.g. `annex-3.xlsx` → `annex-3.md`); without this, Claude can't answer questions about the binary.
- **`team-inputs/` — authored by our side.** Personas, sketches, workshop outputs, debriefs, estimate assumptions. The author owns the file; Claude may help on request but never silently rewrites. Metadata fields (`author`, `role`, `status`, `version`, `informed_by`) tell Claude whose artefact this is and what client evidence fed it. Template in [`team-inputs/README.md`](team-inputs/README.md).

## Metadata, in one minute

Every wiki page and every `team-inputs/` file starts with:

```yaml
---
type: stakeholder        # or persona, competitor, system, adr, epic, risk, clause, …
title: Jane Doe — CISO
tags: [client-x, security, decision-maker]
sources:
  - ../client-inputs/2026-04-17-kickoff-transcript.md
last_updated: 2026-04-20
status: active           # draft | active | approved | superseded
---
```

You never write it by hand — `/ingest` and any *"scaffold a …"* request fill it in. Full field list in [`AGENTS.md`](AGENTS.md); the why is in [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md#page-metadata).

## Optional next steps

Not configured by default. Add when you're comfortable with the basics.

### Scheduled `/lint`

*"Schedule /lint every Monday at 09:00."* Claude uses the `schedule` skill to drop a report into `project-management/` and (optionally) ping Slack or email.

### MCP-backed ingest

Connectors for Slack, Gmail, Jira, Confluence, Drive, and Calendar ship with Claude. Teach Claude to pull directly: *"Pull yesterday's messages from #client-x into `client-inputs/` and ingest."* Same for Confluence pages, Gmail threads, Drive files. Once you've used a variant twice, promote it to `/ingest-slack`, `/ingest-gmail`, etc.

## Setup

All three roles — PM, PO, Tech Lead — drive this repo through **Claude Code running inside the Claude desktop app**. Not the terminal CLI; not the Claude web app. The Claude app gives Claude Code the project folder, handles git for you, and surfaces its workspace in a panel you keep open beside your other tools. No git or terminal knowledge required — this section is the one-time setup every role does once per laptop.

### 1. Install the Claude app (with Claude Code)

Download the Claude desktop app from [claude.ai/download](https://claude.ai/download) and sign in with your Anthropic account. Once installed, enable **Claude Code** inside the app — [claude.com/claude-code](https://claude.com/claude-code) has the up-to-date per-OS steps. After enabling, Claude Code appears as a panel inside the app; you'll point it at a project folder in step 3.

From here on, every instruction in this README that says *"ask Claude Code …"* means: type the quoted prompt into the Claude Code panel in the Claude app.

### 2. Passwordless GitHub access (one-time per laptop)

Takes about five minutes. Ask Claude Code for each step — **follow Claude's instructions as they appear, then come back here for the next step**:

1. *"Do I already have an SSH key for GitHub?"* — Claude checks `~/.ssh/`. If one exists, skip to step 5.
2. *"Generate an SSH key using my work email."* — empty passphrase is fine on a work laptop.
3. *"Add my SSH key to the macOS keychain."*
4. *"Print my SSH public key and copy it to the clipboard."*
5. At [github.com/settings/ssh/new](https://github.com/settings/ssh/new), paste the key, give it a name (e.g. `Work MacBook`), and save.
6. *"Test my SSH connection to GitHub."* — expect `Hi <username>! You've successfully authenticated…`.

> **Note:** When you run step 2, Claude may itself suggest going to GitHub to paste the key — that's step 5. Follow along, then return here and continue from step 6 to verify the connection.

After this, every `push` / `pull` just works. If you see `Permission denied (publickey)` at any point, ask Claude: *"My SSH connection to GitHub is failing — diagnose it."*

### 3. Get a project onto your laptop

On GitHub, open the repo → **Code → SSH**, and copy the URL. Then, in the Claude app:

1. Click the **`+`** button (top-left of the Claude Code panel) to start a new project.
2. When prompted to choose a folder, navigate to your working directory (e.g. `~/work/`) and select it.
3. Ask: *"Clone `git@github.com:vacuumlabs/<repo>.git` here and open it."*
4. Once cloning finishes, click **`+`** again, navigate into the newly-cloned folder (e.g. `~/work/project-qcp-kb/`), and select it. This sets it as the active working context.

New project from this template: *"Create a new repo from `vacuumlabs/project-template` named `<client>-engagement`, clone it here, and walk me through the kickoff checklist."*

> **Each time you open a new chat**, check the folder shown in the top-left of the Claude Code panel — it should show the project repo, not a previous project. If it's wrong, click `+` and re-select the correct folder before asking anything.

### 4. Commit and push, in plain English

A file lives in one of three places:

| Place | Where | Who sees it |
| --- | --- | --- |
| **Working copy** | Files open on your laptop | Only you, right now |
| **Commit** | Snapshot in `.git/` on your laptop | Only you, until you push |
| **GitHub** | Server copy | Everyone with repo access |

Lifecycle: **edit → commit → push**. Usually: *"commit and push this."*

### 5. What can be undone

| Situation | Undoable? | Ask Claude |
| --- | --- | --- |
| Edited, not committed | Yes, cleanly | *"Undo my uncommitted changes to `<file>`."* |
| Committed, not pushed | Yes, cleanly | *"Revert the last commit"* / *"Remove `<file>` from the last commit."* |
| Push rejected (someone pushed first) | Yes | *"My push was rejected — pull and push my changes."* |
| Pushed | Yes, but the original stays in history | *"Revert the last commit and push the revert."* Claude adds a new commit undoing the old one; history is preserved (feature, not bug). |
| Committed a secret | **Permanent in history.** Rotate first. | Rotate the secret, then ask Claude to remove the file. Assume it's been seen. |

### 6. Safety rails already in place

Two hooks in [`.claude/settings.json`](.claude/settings.json):

- **Auto-sync before every action** — silent `git fetch` + `git pull --ff-only` so your working copy stays current.
- **Force-push to `master` blocked** — force-push silently deletes teammates' work. If you truly need one, remove the guard temporarily.
