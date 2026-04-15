# Project Knowledge Base

This repository is the single source of truth for project knowledge. It replaces the Claude project workspace — query it directly when you need context on the engagement.

## How to use this repo

When a question comes in, identify which artefact category it belongs to and read from the matching directory. If a question spans multiple categories, consult each. If information is missing, say so explicitly rather than inferring — gaps here represent real gaps in project knowledge that should be surfaced and filled.

## Routing

| Topic / question type | Directory |
| --- | --- |
| Contract terms, MD budget, commercials, SOW, billing, legal, engagement metadata | [`contract/`](contract/) |
| Sales context, account management notes, client sentiment, client motivation, stakeholder map, target audience | [`deal-context/`](deal-context/) |
| System design, architecture diagrams, tech stack decisions, ADRs, ARB records, integrations | [`technical-architecture/`](technical-architecture/) |
| Scope definition, work breakdown, estimates, assumptions, change requests | [`scope-and-estimates/`](scope-and-estimates/) |
| Personas, PRDs, user journeys, feature maps, product strategy, domain & competitive briefs, problem framing, competitor analysis | [`product-management/`](product-management/) |
| Roadmaps, timelines, milestones, capacity planning, status reports, project assembly (client-facing outputs and presentations), debriefs, post-mortems | [`project-management/`](project-management/) |
| Meeting notes, user feedback, raw client communications, transcripts | [`client-inputs/`](client-inputs/) |

## Directory details

- **`contract/`** — contractual details, MD budget, commercials, SOW, legal, billing arrangements. Also home to [`contract/engagement.md`](contract/engagement.md), the canonical engagement metadata file.
- **`deal-context/`** — sales and account management inputs, client sentiment, client motivation, stakeholder map, target audience.
- **`technical-architecture/`** — system design, architecture diagrams, tech stack decisions, ADRs, Architecture Review Board (ARB) records, integration specs.
- **`scope-and-estimates/`** — scope definition, work breakdown, estimates, assumptions, change requests.
- **`product-management/`** — personas, PRDs, user journeys, feature maps, product strategy, domain briefs, competitive briefs, problem framing, competitor analysis.
- **`project-management/`** — roadmaps, timelines, capacity planning, status reports, **project assembly** (outputs and presentations delivered to the client), debriefs, post-mortems.
- **`client-inputs/`** — meeting notes, user feedback, raw client communications, transcripts.

## Conventions

- **Markdown for narrative.** Prefer Markdown for anything authored inside the repo.
- **Date-prefix time-bound files (required).** Use `YYYY-MM-DD-*.md` (e.g. `2026-04-13-kickoff-notes.md`) for any meeting note, status update, or other time-bound artefact. The repo has no explicit lifecycle metadata — the latest file for a given topic wins, and Claude relies on the date prefix to pick it.
- **Every binary needs a same-stem summary (required).** For any non-Markdown file (PDF, deck, spreadsheet, image), commit a same-stem `.md` next to it (e.g. `kickoff-deck.pptx` → `kickoff-deck.md`) with a short summary. Without this, Claude can't answer questions about the binary's contents.
- **External systems: snapshot + link.** Decision-grade artefacts that live in Jira, Confluence, Drive, or similar (signed SOW, approved PRD, architecture diagrams) should be snapshotted into the repo *and* linked back to the source. Non-decision-grade material can be pointer-only (title + URL + one-line summary).
- **Engagement metadata.** Canonical engagement metadata (client, project code, start date, engagement type, delivery lead, primary stakeholders) lives in [`contract/engagement.md`](contract/engagement.md).
- **Place by primary question.** When adding a new artefact, put it in the directory that matches the *primary* question it answers, even if it touches several.
