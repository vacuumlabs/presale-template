# Project Knowledge Base

This repository is the single source of truth for project knowledge. It replaces the Claude project workspace — query it directly when you need context on the engagement.

## How to use this repo

When a question comes in, identify which artefact category it belongs to and read from the matching directory. If a question spans multiple categories, consult each. If information is missing, say so explicitly rather than inferring — gaps here represent real gaps in project knowledge that should be surfaced and filled.

## Routing

| Topic / question type | Directory |
| --- | --- |
| Contract terms, MD budget, commercials, SOW, billing, legal | [`contract/`](contract/) |
| Sales context, account management notes, client sentiment, client motivation, stakeholder map | [`deal-context/`](deal-context/) |
| System design, architecture diagrams, tech stack decisions, ADRs, integrations | [`technical-architecture/`](technical-architecture/) |
| Scope definition, work breakdown, estimates, assumptions, change requests | [`scope-and-estimates/`](scope-and-estimates/) |
| Personas, PRDs, user journeys, feature maps, product strategy | [`product-management/`](product-management/) |
| Roadmaps, timelines, milestones, capacity planning, status reports | [`project-management/`](project-management/) |
| Meeting notes, user feedback, raw client communications, transcripts | [`client-inputs/`](client-inputs/) |

## Conventions

- Prefer Markdown for narrative artefacts; keep binary/source files (PDFs, decks, spreadsheets) alongside a short MD summary so they're queryable.
- Use clear, dated filenames (e.g. `2026-04-13-kickoff-notes.md`) for time-bound artefacts.
- When adding a new artefact, place it in the directory that matches the *primary* question it answers, even if it touches several.
