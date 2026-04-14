# Project Knowledge Base

This repository is the single source of truth for project knowledge. It replaces the Claude project workspace — query it directly when you need context on the engagement.

## How to use this repo

When a question comes in, identify which artefact category it belongs to and read from the matching directory. If a question spans multiple categories, consult each. If information is missing, say so explicitly rather than inferring — gaps here represent real gaps in project knowledge that should be surfaced and filled.

## Routing

| Topic / question type | Directory |
| --- | --- |
| Contract terms, MD budget, commercials, SOW, billing, legal | [`contract/`](contract/) |
| Sales context, account management notes, client sentiment, client motivation, stakeholder map, target audience | [`deal-context/`](deal-context/) |
| System design, architecture diagrams, tech stack decisions, ADRs, ARB records, integrations | [`technical-architecture/`](technical-architecture/) |
| Scope definition, work breakdown, estimates, assumptions, change requests | [`scope-and-estimates/`](scope-and-estimates/) |
| Personas, PRDs, user journeys, feature maps, product strategy, domain & competitive briefs, problem framing, competitor analysis | [`product-management/`](product-management/) |
| Roadmaps, timelines, milestones, capacity planning, status reports, project assembly (client-facing outputs and presentations), debriefs, post-mortems | [`project-management/`](project-management/) |
| Meeting notes, user feedback, raw client communications, transcripts | [`client-inputs/`](client-inputs/) |

## Directory details

- **`contract/`** — contractual details, MD budget, commercials, SOW, legal, billing arrangements.
- **`deal-context/`** — sales and account management inputs, client sentiment, client motivation, stakeholder map, target audience.
- **`technical-architecture/`** — system design, architecture diagrams, tech stack decisions, ADRs, Architecture Review Board (ARB) records, integration specs.
- **`scope-and-estimates/`** — scope definition, work breakdown, estimates, assumptions, change requests.
- **`product-management/`** — personas, PRDs, user journeys, feature maps, product strategy, domain briefs, competitive briefs, problem framing, competitor analysis.
- **`project-management/`** — roadmaps, timelines, capacity planning, status reports, **project assembly** (outputs and presentations delivered to the client), debriefs, post-mortems.
- **`client-inputs/`** — meeting notes, user feedback, raw client communications, transcripts.

## Conventions

- Prefer Markdown for narrative artefacts; keep binary/source files (PDFs, decks, spreadsheets) alongside a short MD summary so they're queryable.
- Use clear, dated filenames (e.g. `2026-04-13-kickoff-notes.md`) for time-bound artefacts.
- When adding a new artefact, place it in the directory that matches the *primary* question it answers, even if it touches several.
