---
description: Prepare the Sales Strategy Alignment call agenda or document the outcome into T7
argument-hint: "prepare" or "document"
---

# P3f — Sales Strategy Alignment

AI-assisted preparation and documentation for the T7 Sales Strategy Alignment call.

**Two modes — specify which to run:**
- `/p3f prepare` — generates a meeting agenda and key questions for the Sales + CDO + PM call
- `/p3f document` — takes call notes as input, produces the completed T7 template

The call itself is a human conversation. This command prepares for it and captures it — it does not replace it.

**When:** After P3e (Business Architecture) is complete, before the G2 gate. T2 and T3 deals only.

---

## /p3f prepare

Generate a meeting agenda and focused questions for the 30-minute Sales Strategy Alignment call.

### 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `product-management/problem-statement.md` (P3a)
- `product-management/business-architecture.md` (P3e)
- `product-management/competitive-positioning.md` (P2c)
- Budget signal is confirmed or has been explicitly escalated

If any are missing, stop and list what must be done first.

### 1. Read and synthesise

Read the above pages. Identify:
- The 2–3 most credible positioning angles for this specific client (from competitive-positioning.md and what the problem statement tells us about what the client cares about)
- The key commercial unknowns that Sales must clarify (pricing model, T&M vs fixed-price preference, client budget flexibility)
- The scope boundaries that would make or break a winning bid (what to offer, what to exclude)
- Any competitive threats that need a direct response in the proposal

### 2. Produce the meeting agenda

Format: a pre-filled T7 template with the open questions filled in as prompts for the team to answer during the call. This is not answers — it is the structured questions for the 30 minutes.

Agenda structure:

**Opening (5 min):** Confirm the client situation in one sentence. Does everyone agree on what we're competing for?

**Positioning (10 min):** Which angle(s) should lead? Present 2–3 options based on analysis with a recommended primary angle and the reasoning. Ask Sales to confirm or redirect.

**What we're offering (5 min):** Full delivery, Discovery-first, augmentation, hybrid? Surface the tradeoffs for this specific client.

**Commercial model (5 min):** T&M, fixed-price, hybrid, phased? What does the client expect? What does VL prefer for this type of engagement?

**Competitive stance (5 min):** Who are we likely up against? What objection do we most need to pre-empt?

**Key message (close):** Agree one sentence that should run through the proposal.

Write the agenda to a temporary working note. Do not create a wiki page yet — the agenda is for the call, not the permanent record.

Tell the user:
- The recommended primary positioning angle and why
- The most important question to resolve in the call
- "After the call, run `/p3f document` with your notes to produce the completed T7 brief."

---

## /p3f document

Takes the notes from the Sales Strategy Alignment call and produces the completed T7 brief.

### 0. Required inputs

The user must paste or provide the call notes. Ask: "Please share the notes from the Sales Strategy Alignment call. These can be rough — key decisions and any direct quotes."

Also check that the following exist in the wiki:
- `product-management/problem-statement.md`
- `product-management/business-architecture.md`
- `product-management/competitive-positioning.md`

### 1. Produce the completed T7 brief

Using the call notes plus P3 and P2c context, fill in the T7 Sales Strategy Alignment template.

The completed brief covers:

**Positioning angle** — the agreed primary angle (1–2 from the T7 checklist), with a 1–2 sentence explanation of why this angle for this client.

**What we're offering** — the agreed engagement model.

**What we're not offering** — explicit scope exclusions (things VL is choosing not to compete on or include).

**Deal priority** — Must-win / Strong opportunity / Worth pursuing if it fits, with the agreed rationale.

**Competitive stance** — who VL is most likely up against, the key differentiators to lead with, and the objections to pre-empt.

**Commercial model** — agreed pricing model and any known client pricing expectations or constraints.

**Key message** — the one sentence agreed in the call. This anchors the proposal narrative.

**Phase 0 recommendation** — whether paid Discovery is recommended before committing to full scope, with rationale.

**Sign-off** — record who was on the call and that the brief represents the agreed positioning (do not make up sign-off statuses; mark as "discussed" if formal sign-off was not explicit).

### 2. Create the wiki page

Create `deal-context/sales-strategy-alignment.md`:

```yaml
---
type: concept
title: Sales strategy alignment — <client name>
tags: [sales, strategy, positioning, T7, P3f]
sources:
  - ../team-inputs/T7-sales-strategy-alignment.md
  - product-management/competitive-positioning.md
  - product-management/problem-statement.md
last_updated: <today>
status: active
---
```

Also update (or create) `team-inputs/T7-sales-strategy-alignment.md` with the same content in T7 template format — this is the team-authored primary document; the wiki page is the synthesis.

### 3. Update indexes

- Add entry in root [`index.md`](../../index.md)
- Add entry in `deal-context/README.md` under `## Index`

### 4. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P3f sales strategy alignment — <client name>

Sales Strategy Alignment (T7) documented from call on <date>.
- Created: deal-context/sales-strategy-alignment.md
- Primary angle: <positioning angle>
- Key message: "<one sentence>"
- Commercial model: <T&M / fixed / hybrid>
- Phase 0: <recommended / not needed>
```

### 5. Report back

Tell the user:
- Page created
- The agreed key message (this should run through the entire proposal)
- Whether Phase 0 was recommended
- What to do next: "With T7 complete, the G2 gate can be reviewed. After G2, run `/p4` to begin technical architecture."

Changes are committed and pushed automatically when this turn ends.
