---
description: Draft the ARB pre-sale review submission from accumulated project context, following the canonical Confluence ARB template
argument-hint: (no arguments needed)
---

# P6 — ARB Review Submission

Draft the ARB pre-sale review submission. **The canonical ARB template lives on Confluence** at [page 3109978115](https://vacuum.atlassian.net/wiki/x/A4BeuQ) — there is no copy in this repo. The canonical template covers both Delivery and Pre-sale Architecture Reviews; each section is marked **Required (R)** or **Optional (O)** for each review type.

This command:
1. Fetches the canonical template from Confluence (via the Atlassian MCP) so the draft is always in sync with whatever the ARB community has agreed.
2. Drafts the **Pre-sale Required** sections from accumulated wiki context.
3. Writes the result to `technical-architecture/arb-records/YYYY-MM-DD-arb-submission.md` for the SA to review before the session.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `product-management/problem-statement.md` (P3a)
- `product-management/known-unknowns.md` (P3d)
- `product-management/capabilities-map.md` (P3b)
- `product-management/in-scope-flows.md` (P3c)
- `product-management/business-architecture.md` (P3e)
- `product-management/regulatory-brief.md` (P2b)
- `technical-architecture/vl-assets.md` (P4a)
- `technical-architecture/architecture.md` (P4b)
- `project-management/epics.md` (P5a)
- `project-management/phasing.md` (P5b)
- `project-management/estimation-dev.md` (P5c) — including the `## AI productivity (internal)` subsection
- `project-management/estimation-overhead.md` (P5d)
- `project-management/estimation-final.md` (P5e)

If any of these is missing, stop and list exactly which pages need to be created before P6 can run.

Also confirm that:
- Architecture has been settled (P4b-ii complete, diagrams exist)
- Estimation is reconciled (`estimation-final.md` exists, not just draft parts)

If either is not settled, stop: "P6 should run after architecture and estimation are finalised. Complete P4 and P5 first."

## 1. Fetch the canonical ARB template

Fetch the current canonical template from Confluence:

- Atlassian MCP: `getConfluencePage` with `cloudId: vacuum.atlassian.net`, `pageId: 3109978115`, `contentFormat: markdown`.
- Read the section structure, R/O markers, and per-section question lists.

**If the Atlassian MCP is unavailable:** stop, tell the user the canonical template lives at https://vacuum.atlassian.net/wiki/x/A4BeuQ, and ask them to open it manually before re-running. Do not draft against a stale local copy — there is no local copy by design.

## 2. Draft the ARB submission

Populate the **9 Pre-sale Required (R) sections** of the canonical template: 1, 2, 3, 4, 5, 7, 8, 9, 10. Sections marked Optional for Pre-sale (6 Architectural Decisions, 11 Links in the current version) are included **only if useful for this engagement**.

**Header**

Fill the header table: Name (engagement title), Type = `Presale`, Domain (Regtech/Legal, Banking, Payments, Wealth Management, Lending, or Other — pick from T1 brief), Region (from T1 brief), Presenter (the SA), Date (today).

**Section 1 — Review Context**

- Review type: Pre-sale
- Project / engagement name
- Review date
- Presenter
- What decision does this ARB review need to support?
- Is this a first review or a follow-up?
- 2–4 specific questions the SA wants the ARB to focus on

**Section 2 — Business Context & Goals**

- Who the client is and what business problem they are solving (from `problem-statement.md`)
- Measurable goals and success criteria (pull from §2.4 What Success Looks Like in `problem-statement.md`, and §5.5 Outcomes & Success Measures in `business-architecture.md`)
- Key stakeholders and actors (from `business-architecture.md` §1)
- Constraints: time, budget, regulatory, organisational
- Engagement tier and contract model (T&M / FSFP)
- Budget signal: Hard / Soft / Unknown — amount (from T1 brief)

**Section 3 — Assumptions & Open Questions**

Draw from `known-unknowns.md`. For each assumption: state it, the impact if wrong, mark as **Confirmed / Unverified / High Risk**. For each open question: state it, and the impact if it cannot be answered before delivery begins. This section is especially important for Pre-sale reviews — the ARB will probe assumptions.

**Section 4 — Business View**

Summarise the business capability landscape (from `capabilities-map.md`): key capability domains, scope boundary, key user types, in-scope flows, and how external systems / regulators / partners participate.

**Section 5 — Technical Architecture Overview**

Keep this **conceptual and high-level** — the ARB does not need wire-level detail at the Pre-sale stage.

- Architectural approach and rationale (from `architecture.md` §1)
- Key components and their responsibilities (from §2)
- External integration map (from §3)
- Technology stack and why it fits this engagement (from §4)
- Deployment model / infrastructure (from §5)
- Reference architecture diagrams in `technical-architecture/`

**Section 7 — Risks, Security & Compliance**

Risk table covering technical, commercial, delivery, regulatory, and security risks. For each: description, likelihood (H/M/L), impact (H/M/L), proposed mitigation, residual risk after mitigation. Draw from `architecture.md` §8 risks, `known-unknowns.md`, and `regulatory-brief.md`.

**Section 8 — Reuse & Alignment**

Draw from `vl-assets.md`. Which VL assets are used and how. Which past deals informed this architecture. Are there reusable capabilities being built (integration adapters, workflow engines, compliance modules) that could become shared assets.

**Section 9 — Estimate Drivers**

- Top 5 drivers of effort
- What would cause the estimate to move materially (up / down)?
- Dev effort range from `estimation-dev.md`, overhead from `estimation-overhead.md`, combined and reconciled total from `estimation-final.md`
- Key assumptions the estimate rests on
- Budget reconciliation: how the total compares to the budget signal, or the three-option scenarios if budget was Unknown

> **AI productivity (internal — do not surface to client).** The SA must be ready to defend the AI-tooled productivity assumptions baked into the dev range. These live in the internal `## AI productivity (internal)` subsection of `estimation-dev.md` and are not surfaced in the client-facing proposal (per 2026-05-13 decision). Read them and verify the numbers are realistic before the ARB session.

**Section 10 — Summary & Requested Feedback**

One-paragraph summary of the proposed approach and why it is the right recommendation. 2–4 specific questions for the ARB to focus on. Where does the SA want focused feedback?

**Optional sections — include only if useful**

- **Section 6 — Key Architectural Decisions** (Optional for Pre-sale). Include if material decisions are open or contested — e.g. Thought Machine Vault vs. Mambu — and the SA wants ARB endorsement.
- **Section 11 — Links** (Optional). Include if the ARB benefits from direct pointers to client RFPs, estimation source files, the proposal draft, or LLM wiki entry points.

If a Pre-sale R section legitimately has no content (e.g. no regulatory exposure), state "Not applicable for this engagement" rather than omitting the section header. Be explicit, not silent.

## 3. Create the output file

Write to `technical-architecture/arb-records/YYYY-MM-DD-arb-submission.md` (use today's date), starting with the wiki metadata block:

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

Body: the drafted ARB submission. At the bottom, leave a "After the ARB session" block for feedback / decisions / actions / Confluence record URL.

## 4. After the ARB session

After the session is held and feedback has been incorporated:
1. Update the `status:` field to `approved` (or `revised` if major changes).
2. Save the reviewed version to Confluence under the ARB space — this is the durable record. The repo file is the working draft; Confluence is the archive.

## 5. Update indexes

- Add entry in root [`index.md`](../../index.md)
- Add entry in `technical-architecture/README.md` under `## Index`

## 6. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P6 ARB submission — <client name>

ARB submission draft created (matches canonical Vacuumlabs ARB template at Confluence 3109978115). SA review required before session.
- Created: technical-architecture/arb-records/<date>-arb-submission.md
- Status: draft — 9 Pre-sale R sections populated; optional sections: <included / omitted>
```

## 7. Report back

Tell the user:
- File created at `technical-architecture/arb-records/YYYY-MM-DD-arb-submission.md`
- The 2–4 questions proposed for ARB focus (Section 10)
- Whether optional sections (6 Architectural Decisions, 11 Links) were included, and why
- Any sections where content was thin due to open questions
- Reminder to read the internal AI productivity subsection in `estimation-dev.md` before the ARB
- What to do next: "Once ARB feedback is incorporated, run `/p7` to start proposal assembly."

Changes are committed and pushed automatically when this turn ends.
