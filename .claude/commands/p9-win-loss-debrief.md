---
description: Capture deal outcome in a structured format that feeds the VL knowledge base
argument-hint: (no arguments needed)
---

# P9 — Win/Loss Debrief

Capture the deal outcome in a structured format that feeds Vacuumlabs' knowledge base. Every closed deal is a learning opportunity.

**Run the post-mortem call with the presale team before running this command.** P9 synthesises the team's collective memory — it cannot substitute for it. Fill in `team-inputs/T6-postmortem-call.md` during or immediately after the call, then run `/ingest team-inputs/T6-postmortem-call.md` before running P9.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `team-inputs/T6-postmortem-call.md` — filled in during/after the post-mortem call
- Core project context is in the wiki (at minimum: problem statement, estimation-final.md, proposal draft)

If T6 is missing or empty, stop: "P9 requires completed post-mortem call notes. Fill in `team-inputs/T6-postmortem-call.md` and run `/ingest team-inputs/T6-postmortem-call.md`, then run `/p9`."

## 1. Produce _deal-outcome.md

Using all accumulated project context and the T6 post-mortem notes, produce a structured deal outcome file.

The output goes to the repo root (`_deal-outcome.md`) and is also pushed to the GDrive knowledge base (step 3).

File structure:

```markdown
# Deal Outcome: <CLIENT NAME>

**Client:** <name>
**Market/Region:** <country or region>
**Vertical:** <e.g. digital lending, core banking, payments>
**Deal type:** <T1 / T2 / T3>
**Engagement start:** <date of first client contact>
**Outcome date:** <date deal closed>
**Outcome:** <Won / Lost / No Decision>

---

## What we proposed
One-paragraph summary: what VL recommended, at what cost, on what timeline. Contract model (T&M / Fixed-price / Hybrid).

---

## What we think won / lost it
The 2–3 most likely decisive factors, stated honestly.

For each factor:
- What happened or was said
- Why it mattered (your interpretation)
- Distinguish: things the client told us directly vs. inferred vs. uncertain

---

## What we'd do differently
Concrete, specific changes for next time. Not generic lessons. One point per person from the post-mortem call.

---

## Estimate accuracy
_(Complete if won and partially delivered, or if client shared what they awarded to another vendor)_

- What we estimated: [effort / cost range]
- What the actual scope/budget turned out to be: [if known]
- Key drivers of any variance

---

## Budget signal retrospective
- Budget signal at intake: [Hard / Soft / Unknown], amount if known
- How accurate it turned out to be
- Lessons for handling this type of budget signal in future engagements

---

## Reusable patterns
2–5 genuinely reusable items for future engagements in the same vertical:
- Architecture decisions
- Domain insights (not obvious from public sources)
- Estimation benchmarks
- Proposal approaches that landed well or badly
- Integration or platform gotchas

---

## Proof points generated
Did this presale produce material usable as a case study or reference?
- Yes / No / Potentially (pending client consent)
- If yes: what it is, where it lives, consent status

---

## Linked ARB record
<Confluence URL if available>
```

Write to `_deal-outcome.md` at the repo root:

```yaml
---
type: status
title: Deal outcome — <client name>
tags: [deal-outcome, debrief, P9]
sources:
  - team-inputs/T6-postmortem-call.md
last_updated: <today>
status: active
---
```

## 2. Update root index

Add an entry for `_deal-outcome.md` to the root [`index.md`](../../index.md). Note it explicitly — this file is what future P1b searches look for.

## 3. Push to GDrive knowledge base

Using the Google Drive MCP, copy `_deal-outcome.md` to the GDrive Deals folder (ID: `160UcArBo0aG6eLHWwVkN8ZVrWLl9sdbv`).

Ask the user to confirm the target subfolder: "Which subfolder should this go in? Confirm the client's deal subfolder path in GDrive."

**If the GDrive MCP is unavailable**, note this clearly and tell the user:
> "GDrive copy not completed. Please manually copy `_deal-outcome.md` to the client's subfolder in the VL Deals folder (GDrive ID: `160UcArBo0aG6eLHWwVkN8ZVrWLl9sdbv`). This file feeds future P1b similar-deals searches — it must be in GDrive to be searchable."

## 4. Append to log.md

Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] ingest | P9 win/loss debrief — <client name>

Deal outcome recorded. Outcome: <Won / Lost / No Decision>.
- Created: _deal-outcome.md at repo root
- GDrive: <copied to GDrive / pending manual copy>
- Key lesson: <one sentence summary of the most important reusable insight>
```

## 5. Report back

Tell the user:
- `_deal-outcome.md` created at repo root
- Whether GDrive copy succeeded or needs manual action
- The most important reusable pattern or lesson from this deal
- Reminder: "Update the Confluence ARB record with the deal outcome and estimate accuracy if this deal had an ARB submission."

Changes are committed and pushed automatically when this turn ends.
