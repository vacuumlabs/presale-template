---
description: Review the proposal draft for completeness, technical accuracy, narrative quality, and client language
argument-hint: "sa" or "pm" (run each independently)
---

# P8 — Proposal Quality Check

Review the draft proposal against the full accumulated project context before it is sent to the client.

**Run twice — independently.** P8a is the SA review (technical completeness, estimate accuracy, risk coverage). P8b is the PM review (narrative quality, client language, overall flow). Findings from both are merged before resolving.

Usage: `/p8 sa` for the SA review. `/p8 pm` for the PM review.

**Finding severity:**
- **Critical** — blocks sending (missing scope section, broken or missing estimate, no key assumptions stated)
- **Important** — must be fixed before sending (weak client narrative, uncovered material risk, wrong section numbers, missing UAT/hypercare phases)
- **Minor** — address if time permits (phrasing improvements, completeness of supporting sections, style)

All Critical findings must be resolved before G4. Important findings should be addressed.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `product-management/problem-statement.md`
- `product-management/known-unknowns.md`
- `technical-architecture/architecture.md`
- `technical-architecture/vl-assets.md`
- `project-management/estimation-final.md`
- `product-management/competitive-positioning.md`
- A proposal draft in `project-management/proposal-drafts/` (the most recent one)

If the proposal draft does not exist, stop: "P8 requires a proposal draft. Run `/p7` first."

Read the most recent proposal draft from `project-management/proposal-drafts/`.

---

## P8a — SA technical review

_Run with `/p8 sa`_

The SA must read §4.3 (Vacuumlabs Reuse & Accelerators) and be ready to confirm which claims are accurate before this step runs. Each reuse claim is a commercial commitment.

Produce a structured gap report. For each finding: severity (Critical / Important / Minor), the specific text or section at issue, and a concrete suggestion for how to fix it.

**CHECK 1 — Technical completeness**

- [ ] §3 Scope — in-scope flows named correctly and completely? Out-of-scope explicit?
- [ ] §3.3 Key Assumptions — match known unknowns and estimation uncertainty drivers from `estimation-final.md`?
- [ ] §4.3 Reuse & Accelerators — cross-check each claim against `vl-assets.md`. Mark each: CONFIRMED or NEEDS CHANGE
- [ ] §5 Business Architecture — present? Stakeholders, value streams, RACI, and success measures covered?
- [ ] §6.1 Components — match `architecture.md`?
- [ ] §6.2 External Integrations — all integration points present? Patterns correct?
- [ ] §6.3 Tech Stack — matches `architecture.md`?
- [ ] §6.4 Infrastructure — matches `architecture.md`?
- [ ] §6.5 Diagrams — C4 diagrams included or linked?
- [ ] §7 Delivery Plan — phases, durations, team compositions match `phasing.md`? UAT, go-live, hypercare, handover all present?
- [ ] §9.1 Estimate Summary — effort and cost ranges match `estimation-final.md` exactly?
- [ ] §9.2 Estimate by Phase — UAT, go-live, hypercare, handover all have separate line items?
- [ ] Phase 0 — if recommended in phasing.md, is it scoped and priced?

**CHECK 2 — Known unknowns cross-reference**

Cross-reference `known-unknowns.md`. List any assumptions the proposal relies on that have not been confirmed. Group as High / Medium / Low risk.

**CHECK 3 — Risk coverage**

Cross-reference risks in §10 against `architecture.md` §8 (risks) and `estimation-final.md` uncertainty drivers. Flag any risks identified internally that are absent from the proposal.

**CHECK 4 — Estimate accuracy**

Confirm that `estimation-final.md` numbers are reflected exactly in §9.1 and §9.2. Any discrepancy is Critical. Note whether the three-option scenarios (if budget was Unknown) are handled correctly.

Create `project-management/quality-checks/YYYY-MM-DD-sa-review.md`:

```yaml
---
type: status
title: SA quality review — <client name>
tags: [quality-check, sa, P8]
sources:
  - project-management/proposal-drafts/<draft-filename>.md
last_updated: <today>
status: draft
---
```

Share with PM before merging findings.

---

## P8b — PM narrative review

_Run with `/p8 pm`_

Produce a structured gap report with the same severity levels (Critical / Important / Minor).

**CHECK 1 — Narrative completeness**

- [ ] §1 Executive Summary — stands alone? Covers client challenge, proposal summary, headline timeline, first value delivery, headline cost range, why VL, proposed next step?
- [ ] §2 Understanding Your Challenge — shows genuine understanding of this specific client? References specific things they said?
- [ ] §2.4 What Success Looks Like — specific, measurable outcomes (not "a modern platform")?
- [ ] §4.1 Architectural Approach — explained in plain language a non-technical executive can follow?
- [ ] §4.2 Guiding Principles — each principle connected to a specific client constraint?
- [ ] §8.3 Client-Side Commitments — dedicated UAT resources, go/no-go decision authority, and named handover recipients explicitly stated?
- [ ] §11 Why Vacuumlabs — at least one directly relevant VL reference?
- [ ] §12 Next Steps — clear proposed next action?

**CHECK 2 — Client language**

Flag any passage that:
- Uses VL-internal language the client would not recognise
- Describes VL's process rather than the client's outcome
- Is generic — could apply to any fintech client
- Uses technical language in sections that should be accessible to a business decision-maker

For each flag: quote the problematic text and suggest a rewrite in client language.

**CHECK 3 — Key message consistency**

Read `deal-context/sales-strategy-alignment.md`. Check whether the agreed key message runs through §1, §2, §4, and §11.

**CHECK 4 — "Why Vacuumlabs" check**

Does the proposal give a clear, specific answer to: why VL and not a competitor?
- Specific reference to relevant VL track record?
- Direct connection between VL's expertise and this client's specific problem?
- A differentiated position competitors cannot easily match?

**CHECK 5 — G4 executive sign-off readiness (T2/T3 only)**

Confirm that CDO and Head of Sales are aware of the proposal and have been given a review opportunity. This is the G4 gate requirement for T2/T3. If not confirmed, flag as Important.

Create `project-management/quality-checks/YYYY-MM-DD-pm-review.md`:

```yaml
---
type: status
title: PM quality review — <client name>
tags: [quality-check, pm, P8]
sources:
  - project-management/proposal-drafts/<draft-filename>.md
last_updated: <today>
status: draft
---
```

---

## Merging findings

Once both P8a and P8b are complete, the PM merges the two reports:

Create `project-management/quality-checks/YYYY-MM-DD-quality-check.md` — merged findings sorted by severity. Assign an owner and a resolution status to each finding.

## Update indexes and log

- Add entries for all three quality-check files in root [`index.md`](../../index.md) and `project-management/README.md`

Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] ingest | P8 quality check — <client name>

Proposal quality check complete.
- SA review: <N Critical / N Important / N Minor> findings
- PM review: <N Critical / N Important / N Minor> findings
- Merged: project-management/quality-checks/<date>-quality-check.md
- Action required before G4: <list Critical findings>
```

## Report back

Tell the user:
- All findings by severity
- Critical findings that block sending
- Whether G4 executive sign-off has been confirmed (T2/T3)
- What to do next: "Resolve all Critical findings, address Important ones, then proceed to G4 and send."

Changes are committed and pushed automatically when this turn ends.
