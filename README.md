# Project Template

A template repository for running an AI-assisted SDLC against a real client engagement. It is the project's knowledge base — a structured home for the contract, deal context, architecture, scope, product and project management artefacts, and raw client inputs — queryable by Claude (or any LLM) and by humans.

See [`AGENTS.md`](AGENTS.md) for the routing map of what lives where and the conventions for authoring artefacts.

## Why we are doing this

Client projects generate a lot of context that today lives scattered across Slack, Drive, email, decks, and individual heads. When that context isn't in one place:

- onboarding a new person (or an AI agent) is slow and lossy,
- decisions get re-litigated because nobody can find the previous answer,
- questions from the client take hours of archaeology to answer,
- critical details (a contract clause, an architectural assumption) silently go missing.

Consolidating project knowledge into a single, well-structured, version-controlled place fixes all of that. It also makes the knowledge *queryable* — both humans and LLMs can navigate a predictable folder layout and get grounded answers instead of guesses.

## Why a GitHub repository (and not a Claude project)

We previously kept this material inside a Claude project. Moving it into a Git repository gives us several properties a Claude project can't:

- **Resilience to Claude outages.** The files live outside Claude. If Claude is down or a workspace breaks, the knowledge is still there, still readable, still editable with any tool.
- **Single folder on disk.** Everything the project needs is in one place — clonable, backupable, greppable — instead of split between a Claude workspace and wherever the "real" files live.
- **Full change history.** Git records who changed what and when. Every artefact has an audit trail, blame, and the ability to time-travel to a previous state.

## How to use this repo

Each client engagement gets its own repository, created from this template via GitHub's **Use this template** button. Once created, follow the kickoff checklist, then use the repo throughout delivery, and archive it at close-out.

### Kickoff checklist

- [ ] Rename the new repo to `<client>-engagement` (or the current team convention) and update its description.
- [ ] Fill in [`contract/engagement.md`](contract/engagement.md) with client, project code, start date, engagement type (FSFP / T&M), delivery lead, and primary stakeholders.
- [ ] Configure GitHub access — invite the delivery team and set branch protection on `main` if desired.

### Running the engagement

- Add or update artefacts in the directory that matches their topic; see [`AGENTS.md`](AGENTS.md) for routing and conventions.
- Claude is the primary interface — it reads and writes the repo directly, using `AGENTS.md` to route questions and ground its answers in the files.
- Humans can edit via the GitHub web UI, via `git`, or by asking Claude to make the change.

### At close-out

- Archive the repo on GitHub when delivery finishes. History stays intact and read-only.
