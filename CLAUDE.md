# Claude Instructions

This repo is a **Vacuumlabs presale engagement** running the V2 presale SDLC. For process guidance, invoke the `vacuumlabs-presale-guide` skill — it knows the V2 stage map, gates, templates, and slash commands.

Before answering any question about this engagement, read [index.md](index.md) and follow the routing and conventions in [AGENTS.md](AGENTS.md).

## Presale slash commands

Slash commands drive each stage of the presale. Run them inside this repo.

| Stage | Command | When |
|-------|---------|------|
| Deal Intake | `/p1` | After T1 brief is in team-inputs/ |
| Domain Research | `/p2` | After G1, before first client call |
| Problem Framing | `/p3` | After discovery call, T2 in team-inputs/ |
| Business Architecture | `/p3e` | After P3d, before G2 (T2/T3) |
| Sales Strategy Alignment | `/p3f prepare` or `/p3f document` | After P3e, before G2 (T2/T3) |
| Technical Architecture | `/p4` | After G2, all P3 + T7 outputs in wiki |
| Estimation | `/p5` | After P4 is settled |
| ARB Review | `/p6` | After P4 + P5, before ARB session |
| Proposal Assembly | `/p7` | After G3 (or ARB waived) |
| Quality Check | `/p8 sa` and `/p8 pm` | After P7, before G4 |
| Win/Loss Debrief | `/p9` | After deal closes, T6 in team-inputs/ |

If a command stops because an input is missing, it will tell you exactly what to add before re-running.

## Auto-routing project questions through the wiki

When the user asks a substantive question about the engagement — client, scope, architecture, stakeholders, estimate, timeline, decisions, risks, or anything else that should be grounded in this repo — follow the workflow in [`.claude/commands/query.md`](.claude/commands/query.md) **without being asked**. That means: read `index.md` first, prefer wiki pages over raw sources, cite every factual claim, name gaps honestly, flag contradictions, offer to refile reusable answers as new wiki pages, and append to `log.md`.

Meta-questions — about this repo's structure, the tooling itself, how to run a command, or casual conversation — do **not** trigger the wiki workflow. Answer those directly.

## External skills and integrations

For project questions, **do not invoke external skills** (Slack, Jira, Confluence, Gmail, Google Drive, or any other MCP-backed integration) unless the user explicitly asks you to. Go directly to `index.md` and the wiki workflow instead. External skills may be used to answer sub-parts of a question when you judge the wiki lacks the needed information and the user would clearly benefit — but the wiki is always the first stop, and reaching out to external systems is the exception, not the default.

When in doubt, ask.
