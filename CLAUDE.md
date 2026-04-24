# Claude Instructions

Before answering any question about this project, read [index.md](index.md) and follow the routing and conventions in [AGENTS.md](AGENTS.md).

## Auto-routing project questions through the wiki

When the user asks a substantive question about the project — client, scope, architecture, stakeholders, commercials, timeline, decisions, risks, or anything else that should be grounded in this repo — follow the workflow in [`.claude/commands/query.md`](.claude/commands/query.md) **without being asked**. That means: read `index.md` first, prefer wiki pages over raw sources, cite every factual claim, name gaps honestly, flag contradictions, offer to refile reusable answers as new wiki pages, and append to `log.md`.

Meta-questions — about this repo's structure, the tooling itself, the demo flow, how to run something, or casual conversation — do **not** trigger the wiki workflow. Answer those directly.

## External skills and integrations

For project questions, **do not invoke external skills** (Slack, Jira, Confluence, Gmail, Google Drive, or any other MCP-backed integration) unless the user explicitly asks you to. Go directly to `index.md` and the wiki workflow instead. External skills may be used to answer sub-parts of a question when you judge the wiki lacks the needed information and the user would clearly benefit — but the wiki is always the first stop, and reaching out to external systems is the exception, not the default.

When in doubt, ask.
