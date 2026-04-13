# Project Template

A template repository for running an AI-assisted SDLC against a real client engagement. It is the project's knowledge base — a structured home for the contract, deal context, architecture, scope, product and project management artefacts, and raw client inputs — queryable by Claude (or any LLM) and by humans.

See [`CLAUDE.md`](CLAUDE.md) for the routing map of what lives where.

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
- **Scope locking for FSFP engagements.** For fixed-scope / fixed-price projects, we can tag or branch the repo at the moment delivery starts, freezing the agreed scope. Any later change is visible as a diff against that tag and can be handled as a change request.
- **Full change history.** Git records who changed what and when. Every artefact has an audit trail, blame, and the ability to time-travel to a previous state.
- **Review and questioning as a first-class workflow.** Pull requests let any input or output be reviewed, commented on, and challenged *before* it becomes canonical. Inputs aren't just dumped in — they're reviewed. Outputs aren't just produced — they're second-opinioned. This is how QA plugs in without creating a competing source of truth.

## How to use this repo

1. Clone it.
2. Add or update artefacts in the directory that matches their topic (see [`CLAUDE.md`](CLAUDE.md)).
3. Raise changes as pull requests so they can be reviewed and questioned.
4. Ask Claude questions against the repo — it uses `CLAUDE.md` to route to the right directory and ground its answer in the files.
