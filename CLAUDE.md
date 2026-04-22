# Claude Instructions

Before answering any question about this project, read [index.md](index.md) and follow the routing and conventions in [AGENTS.md](AGENTS.md).

## Auto-routing project questions through the wiki

When the user asks a substantive question about the project — client, scope, architecture, stakeholders, commercials, timeline, decisions, risks, or anything else that should be grounded in this repo — follow the workflow in [`.claude/commands/query.md`](.claude/commands/query.md) **without being asked**. That means: read `index.md` first, prefer wiki pages over raw sources, cite every factual claim, name gaps honestly, flag contradictions, offer to refile reusable answers as new wiki pages, and append to `log.md`.

Meta-questions — about this repo's structure, the tooling itself, the demo flow, how to run something, or casual conversation — do **not** trigger the wiki workflow. Answer those directly.

When in doubt, ask.
