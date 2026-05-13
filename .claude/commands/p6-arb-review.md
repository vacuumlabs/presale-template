---
description: Draft the ARB pre-sale review submission from accumulated project context
argument-hint: (no arguments needed)
---

# P6 — ARB Review Submission

Draft the ARB pre-sale review submission. By the time this runs, all the source material exists in the wiki. This command produces a first draft that the SA reviews and refines before the ARB session.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `product-management/problem-statement.md` (P3a)
- `product-management/known-unknowns.md` (P3d)
- `product-management/capabilities-map.md` (P3b)
- `product-management/in-scope-flows.md` (P3c)
- `technical-architecture/vl-assets.md` (P4a)
- `technical-architecture/architecture.md` (P4b)
- `project-management/epics.md` (P5a)
- `project-management/phasing.md` (P5b)
- `project-management/estimation-final.md` (P5e)

If any of these is missing, stop and list exactly which pages need to be created before P6 can run.

Also confirm that:
- Architecture has been settled (P4b-ii complete, diagrams exist)
- Estimation is reconciled (`estimation-final.md` exists, not just draft parts)

If either is not settled, stop: "P6 should run after architecture and estimation are finalised. Complete P4 and P5 first."

## 1. Draft the ARB submission

Using the T3 ARB Submission template as a guide, populate all 10 sections. Do not skip sections — if a section has limited content, note what is known and what is open.

**Section 1 — Review Context**
- What decision does this ARB review need to support?
- What is the deadline and why?
- What specific questions does the SA want the ARB to focus on? (2–4 questions)

**Section 2 — Business Context & Goals**
- Who is the client and what do they do?
- What are they trying to achieve, in business terms?
- Engagement tier, contract model, budget signal.

**Section 3 — Assumptions & Open Questions**
Draw from `known-unknowns.md`. For each assumption: state it, the impact if wrong, and mark as Confirmed / Unverified / High Risk.

**Section 4 — Business View**
Summarise the client's business capability landscape: key capability domains, scope boundary, key user types.

**Section 5 — Technical Architecture Overview**
- Architectural approach and rationale
- Key components and their responsibilities
- External integration map
- Key integration patterns
- Reference the architecture diagrams (note their location in the repo)

**Section 6 — Risks, Security & Compliance**
Risk table: technical, commercial, delivery, regulatory risks. For each: description, likelihood (H/M/L), impact (H/M/L), mitigation.

**Section 7 — Reuse & Alignment**
Draw from `vl-assets.md`. Which VL assets are used, how they are applied, estimated effort saving. Which past deals informed this architecture?

**Section 8 — Estimate Drivers**
- Top 5 drivers of effort
- What would cause the estimate to move materially?
- Dev effort range from `estimation-dev.md`, overhead from `estimation-overhead.md`, combined total from `estimation-final.md`

**Section 9 — Summary & Requested Feedback**
One-paragraph summary of the proposed approach. 2–4 specific questions for the ARB to focus on.

Create `technical-architecture/arb-records/YYYY-MM-DD-arb-submission.md` (use today's date):

```yaml
---
type: adr
title: ARB submission — <client name>
tags: [arb, architecture, review]
sources:
  - ../architecture.md
  - ../../product-management/known-unknowns.md
  - ../../project-management/estimation-final.md
last_updated: <today>
status: draft
---
```

## 2. After the ARB session

After the ARB session is held and feedback has been incorporated:

1. Update the status field in the file to `approved` (or `revised` if major changes were made)
2. Save the final reviewed version to Confluence (ARB space at vacuum.atlassian.net) — this is the durable record. The repo file is the working draft; Confluence is the archive.

Tell the user after drafting:
> "Review this draft carefully — particularly Section 3 (Assumptions) and Section 6 (Risks). Expect 30–60 minutes of SA review before it is ready for the ARB session. After the ARB, update this file with the outcome and save the final version to Confluence."

## 3. Update indexes

- Add entry in root [`index.md`](../../index.md)
- Add entry in `technical-architecture/README.md` under `## Index`

## 4. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P6 ARB submission — <client name>

ARB submission draft created. SA review required before session.
- Created: technical-architecture/arb-records/<date>-arb-submission.md
- Status: draft — <N> sections populated, key open areas: [note any thin sections]
```

## 5. Report back

Tell the user:
- File created at `technical-architecture/arb-records/YYYY-MM-DD-arb-submission.md`
- The 2–4 questions proposed for ARB focus
- Any sections where content was thin due to open questions
- What to do next: "Once ARB feedback is incorporated, run `/p7` to start proposal assembly."

Changes are committed and pushed automatically when this turn ends.
