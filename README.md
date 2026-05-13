# Project Template

A template repo for running an AI-assisted SDLC against a client engagement — **across the entire lifecycle**: presales, discovery, scope, architecture, delivery, and close-out. It is the project's **LLM wiki** — raw sources stay immutable, Claude synthesises them into cross-linked pages, and questions read the wiki instead of re-discovering knowledge each time.

**UI: Claude Code inside the Claude desktop app, for everyone.** Not the terminal CLI, not the Claude web app. First-time install + GitHub auth → [`SETUP.md`](SETUP.md).

**Not a code repo.** Code itself lives in a separate repository created at delivery kickoff.

See [`AGENTS.md`](AGENTS.md) for the schema Claude follows (routing, operations, metadata, edit contract). See [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md) for design rationale and deeper mechanics.

> **Running a Vacuumlabs presale?** This repo is the seed for V2 presale engagements. Per-stage slash commands (`/p1`–`/p9`) drive intake, problem framing, technical architecture, estimation, ARB submission, proposal assembly, and win/loss debrief. Jump to [V2 Presale SDLC](#v2-presale-sdlc) below, or read the Confluence overview: [V2 Presale SDLC Overview](https://vacuum.atlassian.net/wiki/spaces/Engineerin/pages/3298721855/V2+Presale+SDLC+Overview) · [V2 Tooling — Presale Template Repo](https://vacuum.atlassian.net/wiki/spaces/Engineerin/pages/3364356100/V2+Tooling+Presale+Template+Repo).

## Who uses this repo

Three roles drive the wiki on a typical engagement. Each section below is self-contained — scroll to yours.

- [**Project Manager (PM)**](#if-youre-the-project-manager) — contract, commercials, stakeholders, roadmap, risks, weekly status
- [**Product Owner (PO)**](#if-youre-the-product-owner) — problem framing, personas, features, scope, estimates
- [**Tech Lead**](#if-youre-the-tech-lead) — architecture, systems, integrations, ADRs, technical scope. _In V2 presale engagements this is the Solution Architect (SA)._

On a V2 presale engagement, a fourth role joins:

- [**Sales (presale only)**](#sales-presale-only) — T1 brief, budget signal, proposal narrative review, commercial sign-off

Then: [the three verbs](#the-three-verbs-ingest-ask-lint) you'll use daily, [V2 Presale SDLC](#v2-presale-sdlc) if this is a presale, and [first-time setup](#first-time-setup).

## If you're the Project Manager

**Your folders:** [`project-management/`](project-management/), [`contract/`](contract/), [`deal-context/`](deal-context/). Full page catalogues live in those folders' own READMEs.

**Raw sources you bring in**

- → `client-inputs/`: signed SOW, client emails, meeting transcripts, steering-committee decks, client-sent annexes
- → `team-inputs/`: sales-call debriefs, pricing assumptions, your estimate-assumption doc

**Daily / weekly rhythm**

- **After each client meeting** — drag the transcript into Claude Code, ingest into `client-inputs/`. From one ingest you see all of the below in the diff for your review — no separate asks:
  - **Decisions** auto-appended to [`decisions.md`](decisions.md). Example input: transcript line *"approve CR-012 at €20K, billable June"* → row with date, amount, and a link back to the transcript paragraph.
  - **Risks** auto-appended to `risks.md`. Example: CTO says *"data residency is a hard line — anything outside the EU is a deal-breaker"* → entry tagged `data-residency`, owner left blank for you to fill, link back to the source.
  - **Contradictions with prior client statements** auto-flagged in [`contradictions.md`](contradictions.md). Example: today's transcript says *"Q4 launch is firm"*, last month's said *"Q3 is committed"* → entry quoting both sides, status `Resolution: ?`.
  - **Stakeholders** auto-profiled or updated with what they said.
- **Weekly** — draft `status/YYYY-MM-DD-status.md` (Claude pulls the last 7 days from `log.md`); Monday: run `/lint`. Status takes minutes; `/lint` catches stale pages and orphan owners before the client does.
- **Before steering committee** — ask Claude for a commercials + client-sentiment summary. Walk in with the framing already drafted instead of assembling it in the cab.

**Prompts you'll use often**

- *"What's the current status across workstreams?"*
- *"Which risks are open and who owns them?"*
- *"Draft a steering committee summary pulling commercials and client sentiment."*
- *"Draft this week's status from the log."*
- *"Generate an onboarding checklist for a new joiner on this engagement."*
- *"Record a decision: approved CR-012 on 2026-05-01, cost 20 MDs."*

## If you're the Product Owner

**Your folders:** [`product-management/`](product-management/); co-owns scope & estimates under [`project-management/`](project-management/). Full page catalogues live in those folders' own READMEs.

**Raw sources you bring in**

- → `client-inputs/`: discovery-interview notes, client-sent PRDs, user-testing recordings, feedback sessions
- → `team-inputs/`: polished personas (often the canonical page — see [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md#how-the-raw-layer-really-works)), problem framing, competitor briefs, workshop outputs

**Daily / weekly rhythm**

- **Discovery interview ends** — drag notes into Claude Code, ingest into `client-inputs/`. From one ingest you see all of the below in the diff:
  - **Pain points and feature mentions** auto-cross-linked to existing personas and PRDs. Example: interviewee says *"approval delays slow our quarterly close"* → linked to the existing entry in `pain-points.md` and to every persona that already raised it.
  - **Contradictions with existing wiki content** auto-flagged in [`contradictions.md`](contradictions.md). Example: your `personas/cfo.md` (authored in `team-inputs/`) describes the CFO as the buyer; today's interviewee says *"the CISO has final sign-off"* → tension lands in `contradictions.md` with both sides quoted.
  - **Open questions auto-resolved or promoted to contradictions.** Example: `open-questions.md` has *"do users need offline mode?"* — today's transcript has *"we work in basement rooms with no signal"* → Claude proposes closing the question with the evidence linked. If the new input *conflicts* with a previously-decided answer, the open question is promoted to a contradiction instead.
- **You write a persona or domain brief** — place in `team-inputs/` (stays yours to iterate). Claude creates a `product-management/personas/<name>.md` with `canonical_source:` pointing at your file. Your doc stays the source of truth; the wiki adds navigation.
- **Workshop outputs land** — drag photos + summary into Claude Code, ingest into `team-inputs/`. Same auto-extraction as discovery interviews; whiteboards stop dying as JPEGs in someone's Drive.
- **Before backlog grooming** — ask which features still need acceptance criteria. Gaps surface at the table, not mid-sprint when a developer hits one.

**Prompts you'll use often**

- *"Which pain points appear across multiple personas?"*
- *"What open questions remain on feature X?"*
- *"List every feature in scope for the current release, with its owner and acceptance-criteria status."*
- *"Which open questions are blocking the current estimate?"*
- *"Walk a new joiner through the product on one page."*
- *"Scaffold a new persona in `team-inputs/` for a senior end-user."*

## If you're the Tech Lead

**Your folders:** [`technical-architecture/`](technical-architecture/); co-owns scope & estimates under [`project-management/`](project-management/). Full page catalogues live in those folders' own READMEs.

**Raw sources you bring in**

- → `client-inputs/`: architecture diagrams, integration specs, security questionnaires the client filled in, SaaS vendor docs
- → `team-inputs/`: your solution sketch, whiteboard photos from architecture workshops, reference-architecture notes, proof-of-concept writeups

**Daily / weekly rhythm**

- **Architecture workshop ends** — drag whiteboard photos / diagram exports into Claude Code, ingest into `team-inputs/`. From one ingest you see all of the below in the diff:
  - **Systems and integrations auto-identified.** Example: a whiteboard photo showing *"Mobile App → Auth-Service → Postgres"* → creates or updates `systems/auth-service.md` and `systems/postgres.md`, with the flow noted on each.
  - **Contradictions with existing pages auto-flagged.** Example: today's diagram shows a synchronous REST call; existing `systems/order-service.md` says the contract is event-driven → tension lands in [`contradictions.md`](contradictions.md) with both sides quoted.
- **Client sends an architecture doc or integration spec** — drag into Claude Code, ingest into `client-inputs/`. Same auto-identification as above, plus **integration metadata auto-populated where the doc has it.** Example: a client-sent OpenAPI spec contains *"SLA: 99.9%, fallback: queue+retry, contact: alice@vendor.com"* → `integrations/<vendor>.md` filled with owner, SLA, failure modes; TODOs where the source is silent. Binary gets its `.md` companion so Claude can answer questions about it later.
- **A decision is imminent** — scaffold an ADR (*context, options, decision, consequences*); ask Claude to cross-link related systems and integrations. The decision survives team turnover; future TLs reconstruct the rationale without hunting the original meeting.
- **Before ARB** — ask Claude for the integration map and the ADRs you need to defend. Pre-prep goes from days to hours; you walk in knowing which decisions are contested.

**Prompts you'll use often**

- *"What technical assumptions break if we swap provider X?"*
- *"List integrations without clear owners."*
- *"Walk the happy path end-to-end across all systems."*
- *"Which decisions in [`decisions.md`](decisions.md) lack a corresponding ADR?"*
- *"List every ADR, with its status and the systems it touches."*
- *"Generate a pre-ARB pack: the integration map, the ADRs I need to defend, and the open technical questions."*

## V2 Presale SDLC

This repo is also the canonical **Vacuumlabs presale template**. For presale engagements, the V2 SDLC adds tier classification (T1 / T2 / T3), a budget signal as a first-class intake input, four review gates (G1–G4), six templates (T1, T2, T4, T5, T6, T7 — the ARB submission template is canonical on Confluence), and one slash command per presale stage.

**Confluence:** [V2 Presale SDLC Overview](https://vacuum.atlassian.net/wiki/spaces/Engineerin/pages/3298721855/V2+Presale+SDLC+Overview) for the process narrative (stages, gates, templates as humans see them). [V2 Tooling — Presale Template Repo](https://vacuum.atlassian.net/wiki/spaces/Engineerin/pages/3364356100/V2+Tooling+Presale+Template+Repo) is the entry point that explains the repo + tooling division of responsibility.

**AI assistance:** Ask the `vacuumlabs-presale-guide` skill (from the [`superpowers`](https://github.com/anthropics/claude-plugins-official) plugin marketplace — install via `/plugin marketplace add anthropics/claude-plugins-official` then `/plugin install superpowers@claude-plugins-official`) for stage-by-stage and role-by-role guidance.

### Sales (presale only)

**Your job:** supply the T1 brief before G1 and the budget signal before solutioning begins. Participate in the Sales Strategy Alignment call (T7) before G2. Review proposal narrative (§1 Executive Summary, §11 Why Vacuumlabs, §12 Next Steps) and co-sign estimates before G4.

**How to start:**

1. Copy `templates/T1-sales-deal-brief.md` → `team-inputs/T1-sales-deal-brief.md`
2. Fill in — especially the budget signal (Hard / Soft / Unknown)
3. Drag the filled file into Claude Code and ingest it
4. Then `/p1` kicks off the intake

**Your critical input:** the budget signal. All solutioning works backwards from this. If you can't get a Hard or Soft signal, say so in T1 — the process has a Lean / Standard / Full three-option path for Unknown. What it cannot handle is silence.

### Presale slash commands

Each presale stage has a slash command. Run inside the engagement repo — the command reads the wiki, drafts outputs, writes to the right folder, and commits automatically.

| Stage | Command | Required inputs in wiki |
|---|---|---|
| Deal Intake | `/p1` | T1 in `team-inputs/` |
| Domain Research | `/p2` | P1 outputs in `deal-context/` |
| Problem Framing | `/p3` | T2 in `team-inputs/`, P2 outputs |
| Business Architecture | `/p3e` | P3a–P3d outputs |
| Sales Strategy Alignment | `/p3f prepare` → call → `/p3f document` | P3e + P2c outputs |
| Technical Architecture | `/p4` | P3a–P3e + T7 outputs |
| Estimation | `/p5` | P4 outputs |
| ARB Submission | `/p6` | P4 + P5 outputs; fetches canonical ARB template from Confluence at runtime |
| Proposal Assembly | `/p7` | All P1–P5 + P3e outputs |
| Quality Check | `/p8 sa` and `/p8 pm` | Proposal draft |
| Win/Loss Debrief | `/p9` | T6 in `team-inputs/` |

If a command stops because an input is missing, it tells you exactly which file to add before re-running.

## The three verbs: ingest, ask, /lint

These drive the wiki regardless of role. **Two are implicit** — natural action triggers them. **Only `/lint` is an explicit slash command.**

**Ingest — drag-and-drop, no slash command needed.** Drag the file (transcript, PDF, spreadsheet, screenshot, deck, image) straight into the Claude Code panel in the Claude app and say *"ingest this into `client-inputs/`"* or *"… into `team-inputs/`"*. Claude places the file, creates a matching `.md` summary if it's a binary, then runs the full ingest workflow: reads the source, discusses takeaways with you, creates or updates 5–15 wiki pages, flags contradictions into `contradictions.md`, updates `index.md` and folder READMEs, appends to `log.md`, and reports a structured diff for your review. Run one source at a time. (`/ingest <path>` is the explicit fallback for files already on disk.)

**Ask — plain English, no slash command needed.** Just ask. Substantive project questions auto-follow a fixed workflow: read `index.md` first, drill into the relevant wiki pages, answer **with citations**, and — if the answer is likely to be re-asked — offer to refile it as a new page. If the wiki lacks the information, Claude says so. Meta-questions about the tooling are answered directly. (No `/query` slash command exists.)

**`/lint` — the one explicit slash command, run weekly.** Scans for unresolved contradictions, stale pages (`last_updated` >14 days on active engagements), orphan pages, missing wiki pages (entities mentioned 3+ times with no page), index drift, provenance holes, binary-companion gaps, overdue TODOs, invalid metadata. Output lands at `project-management/lint-reports/YYYY-MM-DD-lint-report.md`. Explicit because it's deliberate, infrequent, and produces a dated report many teams schedule weekly.

Routing lives in [`CLAUDE.md`](CLAUDE.md); full operation details in [`HOW-IT-WORKS.md`](HOW-IT-WORKS.md#how-claude-keeps-the-wiki-synced). Optional add-ons (scheduled `/lint`, MCP-backed ingest from Slack / Gmail / Jira / Confluence / Drive) live in [`HOW-IT-WORKS.md` § "Recipes you can add later"](HOW-IT-WORKS.md#recipes-you-can-add-later).

## First-time setup

One-time per laptop: install the Claude desktop app with Claude Code, set up SSH for GitHub, clone the repo, learn the commit-push lifecycle and what can be undone. Full walkthrough in [`SETUP.md`](SETUP.md).
