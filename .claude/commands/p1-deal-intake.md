---
description: Run deal intake — analyse the T1 brief, identify gaps, surface similar past deals, produce a pre-meeting brief
argument-hint: (no arguments needed)
---

# P1 — Deal Intake

This command runs immediately after a T1 Sales Deal Brief lands in `team-inputs/`. It produces four wiki pages that set the team up for the first client interaction: a structured gap analysis, a similar-deals lookup, a soft go/no-go flag list, and a consolidated pre-meeting brief.

Follow all steps in order. Do not skip any.

## 0. Check required input

Look for `team-inputs/T1-sales-deal-brief.md`. If it is not there, stop immediately and tell the user:

> **P1 cannot run yet.** Copy `templates/T1-sales-deal-brief.md` to `team-inputs/T1-sales-deal-brief.md`, fill it in, then run `/p1` again.

If the file exists but `## Deal Name` is blank or all fields are empty, stop and tell the user the T1 brief is incomplete.

## 1. Read and internalise the T1 brief

Read `team-inputs/T1-sales-deal-brief.md` in full.

Extract and hold in working context:
- Client name and engagement tier (T1 / T2 / T3)
- What they want to do (the ask, in their words)
- Budget signal: Hard / Soft / Unknown — and the figure if stated
- Timeline pressure and any hard deadline
- Known stakeholders and their roles
- What VL already knows about this client or domain
- What is explicitly unknown or missing
- Sales' current positioning / angle

If the budget signal field is blank or not one of Hard / Soft / Unknown, flag it prominently in your output:

> **Budget signal missing.** Sales must supply this before the intake call — it drives all solutioning. Ask the deal owner to update T1 and re-run `/ingest team-inputs/T1-sales-deal-brief.md`.

## 2. Produce deal-context/intake-gaps.md

Create `deal-context/intake-gaps.md`.

Structure:

```yaml
---
type: concept
title: Intake gap analysis — <client name>
tags: [intake, gaps, T1]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
last_updated: <today>
status: draft
---
```

Body sections:

**What we know** — a concise summary of what T1 tells us: the ask, the business context, the budget signal, the timeline, and VL's angle. One paragraph per topic. Written for a team member who has not read T1.

**What we don't know** — a structured list of specific, answerable questions that must be resolved before or during the first client call. Group under: Business & Commercial, Technical, Stakeholder & Process. For each question, note why it matters (scope, estimate, risk).

**What to watch for** — red flags or early risk signals visible in the T1 brief (e.g. "budget signal is Unknown — risk of being used for market research", "timeline implies go-live in 4 months which is tight for T2 scope", "no named technical contact — we may be talking only to business stakeholders"). Keep to 3–5 signals max.

## 3. Search for similar past deals (P1b)

Search the GDrive Deals folder (ID: `160UcArBo0aG6eLHWwVkN8ZVrWLl9sdbv`) for `_deal-outcome.md` files. Use the Google Drive MCP.

Search terms to try against past deal titles and summaries:
- The engagement domain (e.g. "core banking", "lending", "payments", "KYC")
- The platform or technology named in T1, if any
- The deal tier

For each result returned, read its `_deal-outcome.md` and extract: client/domain (use anonymised label if needed), what was built, outcome (won/lost/no decision), and any reusable pattern or benchmark.

**If the GDrive MCP is unavailable**, create the wiki page anyway with a placeholder:

> _GDrive MCP was not available when this ran. To populate similar deals: search the Deals folder in GDrive (ID: `160UcArBo0aG6eLHWwVkN8ZVrWLl9sdbv`) for `_deal-outcome.md` files with similar domain or platform, extract the key benchmarks, and paste them here._

Create `deal-context/similar-deals.md`:

```yaml
---
type: concept
title: Similar past deals — <client name>
tags: [intake, benchmarks, past-deals]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
last_updated: <today>
status: draft
---
```

Body:
- For each relevant past deal: anonymised label, what was built, outcome, key benchmark or lesson (effort estimate range, major risk encountered, what won or lost it)
- A direct implication for this engagement: what should we do or avoid based on these comparators?

If no similar deals are found, say so clearly and note what the closest available comparator is.

## 4. Produce deal-context/go-no-go-flags.md

Create `deal-context/go-no-go-flags.md`.

```yaml
---
type: concept
title: Go/no-go flags — <client name>
tags: [intake, go-no-go, G1]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
last_updated: <today>
status: draft
---
```

**Important — V2 gates are soft checkpoints, not hard stops.** The flags below are signals, not blockers. The team uses them to have an informed conversation at the G1 gate, not to mechanically approve or reject.

List flags across three categories:

**Green — conditions that support proceeding:**
- Budget signal provided (even if Soft or Unknown — just having a signal is positive)
- VL has done similar work in this domain or platform
- Deal tier matches the scope described
- Named technical contact or access to technical stakeholders

**Amber — conditions that need attention before or during Discovery:**
- Budget signal Unknown — solutioning cannot be scoped until budget is established; Sales should push for at least a Soft signal before the first client call
- Timeline is aggressive relative to scope — flag for PM to pressure-test in Discovery
- No existing VL relationship with client — may affect trust-building approach
- Regulatory or compliance constraints mentioned but not detailed — need scoping in Discovery

**Red — conditions that should prompt an explicit team discussion before committing presale effort:**
- Budget signal Hard and figure is below €100k for a T2 engagement (likely too small to justify the presale process)
- Client has already chosen a competitor and is using VL for benchmarking or market research only — confirm with Sales
- Engagement tier mismatch: T1 brief describes a T3-level scope — escalate before proceeding

Apply these categories to the specific deal based on what T1 says. Do not list every category mechanically — only flags that are actually relevant to this deal.

## 5. Produce deal-context/pre-meeting-brief.md

Create `deal-context/pre-meeting-brief.md`. This is what the team reads immediately before the first client call.

```yaml
---
type: concept
title: Pre-meeting brief — <client name>
tags: [intake, pre-meeting]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
  - deal-context/intake-gaps.md
  - deal-context/similar-deals.md
  - deal-context/go-no-go-flags.md
last_updated: <today>
status: draft
---
```

Format: one page. Sections:

1. **The client and the ask** — two sentences: who they are and what they want.
2. **What we know** — three bullets: the most important things T1 told us.
3. **What we need to find out** — the five most important open questions from `intake-gaps.md`, ranked by importance.
4. **What we bring** — VL's most relevant experience and angle for this client (from `similar-deals.md` and T1's VL angle field).
5. **Go/no-go status** — the most important flag(s) from `go-no-go-flags.md` in one or two sentences.
6. **Suggested call agenda** — five agenda items for the first client call, ordered to build rapport before getting to constraints.

Keep this page tight. It should take less than three minutes to read.

## 6. Update indexes

Follow the two-level index rule. For each new page created:
- Add an entry under the correct heading in root [`index.md`](../../index.md)
- Add an entry to `deal-context/README.md` under its `## Index` section

## 7. Append to log.md

Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] ingest | P1 deal intake — <client name>

P1 ran on T1 brief for <client name> (<tier>). Four pages created in deal-context/.
- Created: intake-gaps.md — know/don't-know analysis and red flags
- Created: similar-deals.md — <N> past deals found / GDrive unavailable
- Created: go-no-go-flags.md — <N green / N amber / N red> flags
- Created: pre-meeting-brief.md — consolidated one-pager for first client call
```

## 8. Report back

Tell the user:

- Which pages were created
- The top three open questions from `intake-gaps.md`
- The most important go/no-go flag (if any red or amber flags exist)
- Whether similar past deals were found (and how many)
- What to do next: "Review `pre-meeting-brief.md` before your first call. If anything is wrong or incomplete in `intake-gaps.md`, fix it in `team-inputs/T1-sales-deal-brief.md` and re-run `/ingest` to update. Run `/p2` after Domain Research is kicked off."

Changes are committed and pushed automatically when this turn ends.
