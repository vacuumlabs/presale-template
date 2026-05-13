---
description: Run delivery planning and estimation — epics, phasing, dev estimate, overhead, final combined estimate
argument-hint: (no arguments needed)
---

# P5 — Delivery Planning & Estimation

Produce a phased delivery plan and a defensible effort estimate.

The sequence: break scope into epics → group dev work into phases → estimate dev effort bottom-up → cross-check with team×timeline → estimate overhead roles separately as fixed FTE allocations → combine and reconcile to budget.

**Run sequentially.** Each step must be reviewed before continuing.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `technical-architecture/architecture.md` (P4)
- `technical-architecture/vl-assets.md` (P4)
- `product-management/in-scope-flows.md` (P3c)

If any P4 output is missing, stop: "P5 requires P4 to be complete. Run `/p4` first."

Note the budget signal from the T1 brief. If Unknown, the reconciliation step (P5e) will produce three scenarios.

## 1. P5a — Epic generation

Using the architecture document and in-scope flows, break the full delivery scope into epics.

For each epic:
- **Name** — short, descriptive
- **Scope** — what is included; what is explicitly excluded
- **Flows covered** — reference flows from `in-scope-flows.md`
- **Build / reuse / configure** — note any VL assets applied
- **Key dependencies** — which other epics must precede this?
- **Open questions** — anything unknown that would change scope or effort

After all epics:
- Dependency order (foundational vs. independent)
- Epics with highest uncertainty

Create `project-management/epics.md`:

```yaml
---
type: concept
title: Epics — <client name>
tags: [epics, scope, P5]
sources:
  - ../technical-architecture/architecture.md
  - ../product-management/in-scope-flows.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to review with the PO — confirm epics map to in-scope flows and cover no unintended scope. Accept before continuing.**

## 2. P5b — Phased delivery plan (dev scope only)

**After epics.md is reviewed and accepted.**

Group the development epics into delivery phases. This step covers **development work only** — overhead roles are estimated separately in P5d.

For each phase:
- Phase name and number
- Development epics in this phase
- What is explicitly NOT in scope for this phase
- Duration in months
- Development team composition (engineers, SA — not PM/QA/design yet)
- Key milestones (1–2 per phase)
- Dependencies
- Client value delivered at the end of this phase

After all phases:
- Total timeline (dev phases only)
- Phasing rationale
- Phase 0 recommendation — if paid Discovery is needed before committing to full scope, say so here and explain why
- If budget signal is Unknown: note which phases fit within different budget scenarios (this feeds the P5e three-option output)

**Important:** Always include UAT, go-live & release, hypercare, and handover as explicit phases at the end of the plan. These are not optional.

Create `project-management/phasing.md`:

```yaml
---
type: concept
title: Phased delivery plan — <client name>
tags: [phasing, delivery, P5]
sources:
  - project-management/epics.md
  - ../technical-architecture/architecture.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to review with PM. Accept before continuing.**

## 3. P5c — Development effort estimate (bottom-up)

**After phasing.md is reviewed and accepted.**

For each development epic, estimate effort independently of the team×timeline constraint.

For each epic:
1. Complexity: Low / Medium / High / Very High
2. Effort range [min–max man-days]
3. Key assumptions (off-the-shelf vs custom, VL reuse applied, integration complexity, AI-tooled productivity baseline)
4. Confidence: High / Medium / Low, with reason

After all epics:
- Total dev effort range: [min]–[max] man-days
- Top 3 uncertainty drivers
- Assumptions that most affect the total

**Do not produce a single number. The range is the output.**

This is a development-only estimate. Do not include PM, QA, design, or infra in this step.

### SA verification — AI productivity assumptions

Before accepting the dev estimate, the SA must explicitly state and validate the AI-tooled productivity assumptions baked into the man-day ranges. Examples:

- "AI tooling (Claude Code, Copilot, AI test generation) reduces hand-written backend code by 25–35%"
- "Generic infra scaffolding is ~50% faster with AI assistance"
- "Test-suite drafting from prompts cuts QA effort by 20%"

These assumptions stay **internal** — they are not surfaced to the client (per the 2026-05-13 decision: a single estimate, no baseline-vs-AI split). They must, however, be **explicit in the wiki** so the estimate can be defended at ARB and updated if AI tooling capabilities change.

Record the AI productivity assumptions in `estimation-dev.md` under a dedicated `## AI productivity (internal)` subsection. **The SA is responsible and accountable** for these numbers being realistic — the prompt will draft them, but the SA must double-check every line before accepting the estimate.

Create `project-management/estimation-dev.md`:

```yaml
---
type: concept
title: Development effort estimate — <client name>
tags: [estimation, development, P5]
sources:
  - project-management/epics.md
  - project-management/phasing.md
last_updated: <today>
status: draft
---
```

Include both the epic-level breakdown and the cross-check by team×timeline for development (to validate consistency):

**Cross-check — team × timeline (dev only):** For each phase, calculate FTEs × duration × 20 working days. Compare to the bottom-up total. If the gap exceeds 30%, identify the cause and note it.

This cross-check is internal validation. Both methods stay in `estimation-dev.md`.

**Stop here. Ask the user to review estimation-dev.md before continuing.**

## 4. P5d — Overhead role allocations

**After estimation-dev.md is reviewed and accepted.**

Estimate overhead roles as **fixed FTE allocations per phase** — not as percentages of dev effort. Overhead roles: PM, product management (PO/BA), QA, design, infra/DevOps.

For each overhead role:
- Is this role needed for this engagement? (Some may not be — e.g. design may be minimal for a backend-only build)
- For each phase where the role is active: FTE allocation (can be fractional — e.g. 0.3 FTE PM is typical for smaller engagements; 0.5–1 FTE for larger ones)
- Total man-days per role: FTE × phase duration in months × 20 working days/month
- Brief rationale for the allocation size

Use judgment. For a 3–4 dev FTE engagement, a PM at 0.2–0.5 FTE is typical. A 6+ FTE engagement may need 0.5–1 FTE PM. QA is often 0.3–0.5 FTE through delivery and 1 FTE during UAT. Design varies heavily by engagement type.

After all overhead roles:
- Total overhead man-days by role and by phase
- Overhead as a proportion of total dev effort (for sanity check only — this is not how overhead is calculated)

Create `project-management/estimation-overhead.md`:

```yaml
---
type: concept
title: Overhead role allocations — <client name>
tags: [estimation, overhead, P5]
sources:
  - project-management/phasing.md
  - project-management/estimation-dev.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to review overhead allocations before continuing.**

## 5. P5e — Combined estimate and budget reconciliation

**After estimation-overhead.md is reviewed and accepted.**

Combine dev effort and overhead to produce the final estimate.

**1. Phase-level combined estimate**

For each phase: dev man-days (from estimation-dev.md) + overhead man-days per role (from estimation-overhead.md) = total man-days. Include UAT, go-live, hypercare, and handover phases.

| Phase | Dev (man-days) | PM | PO/BA | QA | Design | Infra | Total |
|-------|---------------|----|----|----|----|-----|-------|

**2. Total estimate**

Present: total effort range (min–max man-days) and indicative cost range (€). Cost range = effort × VL blended day rate (ask the user for the current rate if not in the wiki).

**3. Budget reconciliation**

If budget signal is Hard: check whether the estimate fits. If it exceeds budget, propose scope reduction or phasing options.

If budget signal is Soft: check whether the estimate falls within the stated range. Note any variance.

If budget signal is Unknown: produce three scenarios:
- **Lean** — minimum viable scope, smallest team, fastest phases
- **Standard** — recommended approach, full scope
- **Full** — extended scope or enhanced timeline with more parallel work

For each scenario: total man-days, indicative cost, what is included vs. deferred.

**4. Key assumptions**

The 3–5 assumptions the estimate rests on. Be direct about what would cause the number to move up or down.

Create `project-management/estimation-final.md`:

```yaml
---
type: concept
title: Final estimate — <client name>
tags: [estimation, final, P5]
sources:
  - project-management/estimation-dev.md
  - project-management/estimation-overhead.md
  - project-management/phasing.md
last_updated: <today>
status: draft
---
```

## 6. Update indexes

For each new page:
- Add entry in root [`index.md`](../../index.md)
- Add entry in `project-management/README.md` under `## Index`

## 7. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P5 estimation — <client name>

P5 complete for <client name>. Estimation pages created in project-management/.
- Created: epics.md — <N> development epics
- Created: phasing.md — <N> phases, <total timeline>
- Created: estimation-dev.md — dev effort <min>–<max> man-days
- Created: estimation-overhead.md — overhead allocations by role and phase
- Created: estimation-final.md — combined estimate <min>–<max> man-days / €X–€Y
```

## 8. Report back

Tell the user:
- All pages created
- The total estimate range from `estimation-final.md`
- Whether budget reconciliation identified a gap (and if so, the proposed approach)
- Whether three-option scenarios were produced
- What to do next: "Run `/p6` to prepare the ARB submission. If ARB is not required for this tier, proceed to `/p7`."

Changes are committed and pushed automatically when this turn ends.
