---
description: Draft all sections of the proposal document from accumulated project context
argument-hint: (no arguments needed)
---

# P7 — Proposal Assembly

Draft the client-facing sections of the proposal document from all accumulated project context. P7 translates it into proposal language: clear, client-centric, and free of internal VL framing.

P7 does not produce a finished proposal. It produces draft text that the PM assembles into the Google Doc (following the T5 structure) and the SA and Sales review before P8.

Run in four parts, sequentially. Each part feeds the next.

**Never mention Claude or AI tools in the draft text.** The proposal must read as if written by a human team.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- P3 outputs: `problem-statement.md`, `capabilities-map.md`, `in-scope-flows.md`, `known-unknowns.md`
- P3e output: `product-management/business-architecture.md`
- P4 outputs: `technical-architecture/vl-assets.md`, `technical-architecture/architecture.md`
- P5 outputs: `project-management/epics.md`, `project-management/phasing.md`, `project-management/estimation-final.md`
- P2 outputs: `product-management/domain-research.md`, `product-management/competitive-positioning.md`
- `deal-context/sales-strategy-alignment.md` (T7) — for narrative tone and key message

If any of these is missing, stop and list exactly what is needed.

After checking, read `deal-context/sales-strategy-alignment.md` and note:
- The agreed key message (the one sentence that should pervade the proposal)
- The primary positioning angle
- Any objections to pre-empt

## 1. P7a — Technical sections

Produces: §3 Scope, §6 Technical Architecture, §8 Team & Ways of Working, §10 Risks & Mitigations.

Run this first. The narrative sections (P7c) depend on the substance being settled.

**Section 3 — Scope**

3.1 In Scope — list flows and capability domains. One sentence per flow on what VL is building or changing. Reference flows by name from `in-scope-flows.md`.

3.2 Out of Scope — explicit list of what is not included. Include adjacent capabilities that came up in discovery but were deprioritised.

3.3 Key Assumptions — the assumptions this proposal rests on. If any assumption is wrong, scope or estimate may change. Draw from `known-unknowns.md` and `estimation-final.md`.

**Section 6 — Technical Architecture**

6.1 System Components — table of key components, responsibilities, and which in-scope flows they support.

6.2 External Integrations — third parties and existing client systems: what each is, data direction, integration pattern, notable constraints or risks.

6.3 Technology Stack — chosen technologies per layer with one-line rationale.

6.4 Infrastructure — deployment model, cloud provider and region, environments. Note data residency or regulatory constraints.

6.5 Architecture Diagrams — reference the C4 diagrams in `technical-architecture/` with relative links.

**Section 8 — Team & Ways of Working**

8.1 Proposed Team — core team composition for primary delivery phases. For each role: title, responsibilities, FTE allocation.

8.2 Ways of Working — sprint cadence, ceremonies, reporting, escalation. 1 paragraph.

8.3 Client-Side Commitments — what VL needs from the client for delivery to succeed: stakeholder access, timely decisions, system access, participation in key ceremonies. Specifically for closing phases: dedicated UAT resources, go/no-go decision authority, named handover recipients.

**Section 10 — Risks & Mitigations**

10.1 Risk table — draw from `architecture.md` §8 (risks) and `known-unknowns.md`. Cover: technical, delivery, commercial, regulatory risks. For each: likelihood (H/M/L), impact (H/M/L), mitigation.

10.2 Open Questions — questions that are material to scope, timeline, or commercial terms that are not yet resolved. Transparency here protects VL and signals good faith.

**SA must review P7a output before it goes into the Google Doc.**

## 2. P7b — Delivery plan and estimate

Produces: §7 Delivery Plan, §9 Estimate.

**Numbers must match `estimation-final.md` exactly — do not round or adjust.**

**Section 7 — Delivery Plan**

7.1 Phases Overview — summary table following T5 structure. Always include UAT, go-live & release, hypercare (2–4 weeks), and handover as explicit phases.

| Phase | Scope summary | Duration | Team size | Client value delivered |
|-------|--------------|---------|----------|----------------------|

7.2 Phase Detail — for each phase (including closing phases): epics in scope, what is not in scope, team composition, key milestones, dependencies, client value at phase end.

7.3 Timeline — tabular timeline from project start to handover completion. Highlight: start date, first live product, go-live, end of hypercare.

**Section 9 — Estimate**

9.1 Estimate Summary — draw from `estimation-final.md`:

| | |
|---|---|
| **Total effort** | X–Y man-days |
| **Indicative cost** | €X–€Y |

9.2 Estimate by Phase — phase-by-phase breakdown (effort and indicative cost). Include UAT, go-live, hypercare, and handover.

9.3 Key Assumptions — the 3–5 assumptions the estimate rests on.

9.4 What Is Not Included — items excluded from the estimate the client may expect (third-party licensing, infrastructure run costs, client-side change management).

9.5 Post Go-Live — indicative maintenance and support model, if relevant.

**SA and Sales must both review P7b before it goes into the Google Doc — numbers are commercial commitments.**

## 3. P7c — Narrative sections

**After P7a and P7b are settled.**

Produces: §1 Executive Summary, §2 Understanding Your Challenge, §4 Our Approach, §11 Why Vacuumlabs, §12 Next Steps.

Write in clear, client-facing language. Use their language, not VL's. Reference specific things they said in discovery — do not use generic fintech framing. The key message from `sales-strategy-alignment.md` should run through all narrative sections.

**Section 2 — Understanding Your Challenge**

2.1 Your Situation — client's business context. Written in their language, not VL's. Reference specific things they said.

2.2 The Core Challenge — the underlying business problem beneath the surface request.

2.3 Key Tensions — the competing constraints they are navigating. Name them directly.

2.4 What Success Looks Like — specific, measurable outcomes in their terms (e.g. "able to launch a new product in 2 weeks", not "a modern platform").

**Section 4 — Our Approach**

4.1 Architectural Approach — plain language summary of what VL is building and why this is the right approach. No component names or tech stack. Readable by a non-technical executive.

4.2 Guiding Principles — 3–5 principles that shaped the solution, each connected to a specific client constraint or goal from §2.

4.3 Vacuumlabs Reuse & Accelerators — draw from `vl-assets.md`. Explain what VL is bringing and what it means for the client in plain terms.

> **SA must review §4.3 carefully. Reuse claims are a commercial commitment.**

**Section 11 — Why Vacuumlabs**

11.1 Relevant Experience — 2–3 past engagements directly relevant to this client's challenge.

11.2 Domain & Technical Expertise — specific depth VL brings for this engagement.

11.3 How We Are Different — honest differentiators vs. likely competition. Draw from `competitive-positioning.md`.

**Section 12 — Next Steps**

Proposed timeline to contract, what VL needs from the client to proceed, VL contact for follow-up.

**Section 1 — Executive Summary** (write this last)

Cover in 1 page maximum: client challenge (2–3 sentences in their language), what VL is proposing, headline delivery plan and first value delivery date, headline estimate range (from `estimation-final.md`), why VL, proposed next step.

**Review with Sales before copying into the Google Doc.**

## 4. P7d — Business Architecture section

**After P7a, P7b, and P7c are settled.**

Produces: §5 Business Architecture.

**Source:** `product-management/business-architecture.md` (P3e)
**Owner:** PO drafts; SA reviews; PM owns RACI accuracy.

The substance is already in `business-architecture.md` from P3e. P7d adapts it for the proposal:

- Confirm stakeholders, value streams, RACI, operating model impact, and success measures are still accurate against the latest scope
- Tighten wording for the client — drop internal VL phrasing, keep client-facing nouns
- Include a half-page diagram for the headline value stream (Mermaid or reference the `.drawio` from P3b)

**Section 5 — Business Architecture**

5.1 Stakeholders & Actors — stakeholder table + actor list + simple context diagram (from `business-architecture.md` §1).

5.2 Value Streams — 3–5 end-to-end value streams. Summary table + half-page flow for the headline stream. Cross-reference §3 In-Scope Flows.

5.3 Responsibility Split (RACI) — at workstream / capability cluster level. Parties: Bank / VL / Platform Vendor / Other. Flag any RACI ambiguities — these are scope risks.

5.4 Operating Model Impact — optional, transformational deals only. Teams affected, new internal capabilities needed, training/change management at headline level. Cross-reference §8.3 Client-Side Commitments.

5.5 Outcomes & Success Measures — 3–5 measurable outcomes tied to in-scope capabilities. Format: capability change → operational metric → business outcome. Pulls from §2.4 (What Success Looks Like).

**PM must confirm RACI accuracy before this goes into the Google Doc. SA must confirm the headline value-stream diagram.**

## 5. Create the proposal draft file

Create `project-management/proposal-drafts/YYYY-MM-DD-proposal-draft.md` (use today's date):

```yaml
---
type: status
title: Proposal draft — <client name>
tags: [proposal, draft, P7]
sources:
  - product-management/problem-statement.md
  - technical-architecture/architecture.md
  - project-management/estimation-final.md
  - deal-context/sales-strategy-alignment.md
last_updated: <today>
status: draft
---
```

Body: all draft text from P7a, P7b, P7c, and P7d assembled in T5 section order.

## 6. Update indexes

- Add entry in root [`index.md`](../../index.md)
- Add entry in `project-management/README.md` under `## Index`

## 7. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P7 proposal assembly — <client name>

Proposal draft created in project-management/proposal-drafts/.
- Created: <date>-proposal-draft.md — all T5 sections drafted
- P7a: §3 Scope, §6 Tech Architecture, §8 Team, §10 Risks
- P7b: §7 Delivery Plan, §9 Estimate
- P7c: §1 Executive Summary, §2 Challenge, §4 Approach, §11 Why VL, §12 Next Steps
- P7d: §5 Business Architecture
```

## 8. Report back

Tell the user:
- File created at `project-management/proposal-drafts/YYYY-MM-DD-proposal-draft.md`
- Which reviewer must check which sections (SA: §4.3 and §6; Sales: §1, §11, §12; SA+Sales: §9; PM: §5 RACI)
- What to do next: "Assemble the draft text into the Google Doc following T5 structure. Then run `/p8` before sending."

Changes are committed and pushed automatically when this turn ends.
